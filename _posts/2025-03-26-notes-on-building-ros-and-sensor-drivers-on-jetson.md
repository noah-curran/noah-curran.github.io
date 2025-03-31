---
layout: post
title:  "Notes On: Building ROS and Sensor Drivers on Jetson"
date:   2025-03-26 16:44
categories: embedded
---
To my surprise, installing ROS and sensor drivers on Jetson (mine is an NX Orin 16GB version) is not as straightforward as I would have hoped. Here are some notes on my experience with actually getting it to work while installing a Jetson board to be used with our AgileX Hunter 2 which is equipped with the inDro Robotics Commander mount. The inDro Commander mount came equipped with ROS1 and without a GPU, so we wanted to upgrade it using a Jetson board. I had a lot of troubleshooting and failed attempts at getting this to work, so here are some notes to get things deployed quickly.

## Environment Setup
We are using the Isaac ROS (release v3.2) framework[^1] since it comes with convenient Docker containers[^2] for running ROS and some sensor drivers on Jetson devices. There are steps for flashing the Jetson platform[^3] and then setting up the development workspace on the Jetson[^4].

## Setting up RealSense 435i
We follow the instructions in this guide [^5]. However, the drivers won't work and we need to do some fixes, which involve installing librealsense manually. There is a guide for accomplishing this in this page [^6]. We want to follow `Solution 2`. Install script should have `/opt/realsense/build-librealsense.sh -n -j 6 -v v2.55.1`. At this point, run `realsense-viewer` and confirm that the IR, depth, and RGB camera viewers are working. We can also edit `${ISAAC_ROS_WS}/src/isaac_ros_common/docker/Dockerfile.realsense` so that the script above is modified with the `-n` parameter. This makes deploying the Docker easier later so that you don't need to reinstall librealsense every time. Apparently, the reason this happens is because there can be issues with the GPU connecting with librealsense which causes the RealSense drivers to malfunction.

## Setting up RoboSense LIDAR
You need to download[^9][^10] into `${ISAAC_ROS_WS}/src`:
```bash
cd ${ISAAC_ROS_WS}/src
git clone https://github.com/RoboSense-LiDAR/rslidar_sdk
cd rslidar_sdk
git submodule init
git submodule update
git clone https://github.com/RoboSense-LiDAR/rslidar_msg
```
Then edit `rslidar_sdk/config/config.yaml` so that:
```yaml
// common
lidar:
  - driver:
      lidar_type: RSHELIOS_16P
// ...rest of file...
```
To test, run the Docker and build:
```bash
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && ./scripts/run_dev.sh
colcon build --symlink-install --packages-up-to rslidar_sdk
source install/setup.bash
ros2 launch rslidar_sdk start.py
```
At this point you should see a pointcloud in rviz. If you don't, check your network (probably eth0) over Wireshark to make sure that packets are arriving. If they aren't try restarting your Jetson board and try again from the start.

## Setting up AgileX Hunter 2 Drivers
Before getting started with this, we need to enable USB-to-CAN interface. Since the Jetson device has a dedicated CAN interface, these are disabled by default. The guide found in [^11] has instructions on how to enable this without reflashing the Jetson device.

```bash
uname -r

# Download kernel sources which match this version. For Jetpack 6.0 this is r363
cd ~/Downloads
wget https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/sources/public_sources.tbz2

# Extract sources
tar -xvf public_sources.tbz2
cd ~/Linux_for_Tegra/source
tar -xvf kernel_src.tbz2

# Build menuconfig to edit source config
cd kernel/kernel-jammy-src
zcat /proc/config.gz > .config
make oldconfig
sudo apt-get update && sudo apt-get install build-essential libncurses5-dev bc libssl-dev

# In this menu do:
# Networking support → CAN bus subsystem support → CAN Device Drivers → CAN USB interfaces → Geschwister Schneider UG (gs_usb)
# Press M, then save and exit
make menuconfig

# EDITED FROM ORIGINAL VERSION TO SAVE TIME
# Build modules and add gs_usb to kernel drivers
make modules_prepare
make M=drivers/net/can/usb -j4
find . -name "gs_usb.ko" # Probably at drivers/net/can/usb/gs_usb.ko
sudo mkdir -p /lib/modules/$(uname -r)/kernel/drivers/net/can/usb/
sudo cp drivers/net/can/usb/gs_usb.ko /lib/modules/$(uname -r)/kernel/drivers/net/can/usb
sudo depmod -a

# Load gs_usb
sudo modprobe gs_usb
# Verify it is loaded
lsmod | grep gs_usb
# Make it load at boot
echo "gs_usb" | sudo tee -a /etc/modules
```

If everything runs fine then we are good to go forward with setting up the AgileX Hunter 2 ROS drivers.

To set up the drivers, we follow the documentation from the official Github repository[^12].
```bash
cd ${ISAAC_ROS_WS}/src
git clone https://github.com/agilexrobotics/ugv_sdk
git clone https://github.com/agilexrobotics/hunter_ros2
cd ugv_sdk/scripts/
# Only on first time
bash setup_can2usb.bash
# Every time you turn power back on
bash bringup_can2usb_500k.bash
# Test that you get messages over the CAN, make sure robot is turned on and connected over CAN-to-USB
candump can0
# Build and launch nodes
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && ./scripts/run_dev.sh
colcon build --symlink-install --packages-up-to hunter_base
source install/setup.bash
ros2 launch hunter_base hunter_base.launch.py
```

**WARNING: AT THIS POINT IF YOU SEND A COMMAND TO THE ROBOT TO MOVE AND IT IS HOOKED UP TO STUFF OR AROUND OTHER EQUIPMENT THEN IT MAY CAUSE DAMAGE. PROCEED WITH SENDING MOVEMENT COMMANDS AT YOUR OWN CAUTION AND RISK.**

You can send commands with:
```bash
ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.0}}"
```
And change the values to non-zero floats to give it movement.

## Setting up GPS and RTK ROS2 Drivers
We are using the Fixposition Vision-RTK 2 and the documentation for setting it up is really comprehensive [^13].

To summarize:
```bash
cd ${ISAAC_ROS_WS}/src
git clone --recursive -b 8.0.0 https://github.com/fixposition/fixposition_driver
bash src/fixposition
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && ./scripts/run_dev.sh

# Setup dependencies
sudo apt update
sudo apt install libyaml-cpp-dev libboost-all-dev zlib1g-dev libeigen3-dev linux-libc-dev

# Setup googletest
curl -L https://github.com/google/googletest/archive/refs/tags/v1.13.0.tar.gz -o /tmp/gtest.tar.gz
echo "bfa4b5131b6eaac06962c251742c96aab3f7aa78 /tmp/gtest.tar.gz" | sha1sum --check

mkdir /tmp/gtest
cd /tmp/gtest
tar --strip-components=1 -xzvf ../gtest.tar.gz
cmake -B build -S . \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    -DBUILD_SHARED_LIBS=ON
cmake --build build --parallel 4
# You may need to run sudo here
cmake --install build
rm -rf /tmp/gtest.tar.gz /tmp/gtest

# Build colcon
colcon build --symlink-install --packages-up-to fixposition_driver_ros2
source install/setup.bash
```
At this point you can run `ros2 launch fixposition_driver_ros2 node.launch`. However, you may have issues with connecting. The system has a WiFi AP, which we can connect to in order to do some debugging. Once you connect to it, you can access a web client on `10.0.1.1` and go to `Configuration > Network > Ethernet`. Make sure that `dhcp-server` is connected and using the IP address `10.0.2.1`. If you want to use a different IP address, you'll need to edit `src/fixposition_driver/fixposition_driver_ros2/launch/config.yaml` to reflect the correct IP address. If you are able to connect to the web client on the IP address set then things should be working correctly.

## Setting up YOLO and Visual SLAM
There are two guides we are following: YOLO[^7] and Visual SLAM[^8]. Follow these exactly as they are written to get things working.

[^1]: "Isaac ROS" [(link)](https://nvidia-isaac-ros.github.io/)
[^2]: "Isaac ROS Dockers" [(link)](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/tree/main/docker)
[^3]: [(link)](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html#jetson-platforms)
[^4]: [(link)](https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html)
[^5]: [(link)](https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/realsense_setup.html)
[^6]: [(link)](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_nvblox/isaac_ros_nvblox/troubleshooting/troubleshooting_nvblox_realsense.html)
[^7]: [(link)](https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_object_detection/isaac_ros_yolov8/index.html)
[^8]: [(link)](https://nvidia-isaac-ros.github.io/concepts/scene_reconstruction/nvblox/tutorials/tutorial_realsense.html)
[^9]: [(link)](https://github.com/RoboSense-LiDAR/rslidar_sdk)
[^10]: [(link)](https://github.com/RoboSense-LiDAR/rslidar_msg)
[^11]: [(link)](https://gist.github.com/WT-MM/5f414dfa32aca8adbf5ef8e32a391e30)
[^12]: [(link)](https://github.com/agilexrobotics/hunter_ros2)
[^13]: [(link)](https://docs.fixposition.com/fd/installation-and-usagex)