---
layout: post
title:  "Sharing WiFi With a non-WiFi capable Device Over Ethernet"
date:   2024-09-05 14:54
categories: embedded
---
If you're like me and work with embedded systems at a place with strange enterprise networking policies, then this article is for you.

1) For some reason you cannot connect your embedded board over WiFi. Maybe your board doesn't have a WiFi chip, maybe it does and you don't have the right driver, or maybe you have a WiFi chip but can't use it because your network blocks embedded devices from connecting to it.
2) You need the internet to download updates and are too lazy to grab a usb or transfer files from a workstation that is connected to the internet over WiFi (or maybe something else IDK).

Your setup might look something like this:
![The Setup](/assets/images/setup.jpg)

We will call the device with WiFi capabilities our "WiFi machine" and our other device our "non-WiFi machine". Prepare our setup by connecting the two devices with an ethernet cord.
When I did this setup, my WiFi machine is Ubuntu 22.04 and my non-WiFi machine is Ubuntu 22.04.

## On the WiFi machine
1. Go to Settings > Network > Wired > Click the gear icon on one of your wired setup configurations. At this point, you can create a new one if you'd like by pressing "+" next to "Wired".
2. A dialog pops up. Press IPv4 > Shared to other computers > Apply.
3. Click the profile you just updated (to refresh it) and take note of the IPv4 address. Mine is `10.42.0.1`.
4. Restart network services in terminal using `sudo systemctl restart NetworkManager`
5. In your terminal run `ip a`. Locate your ethernet connection, which should be something like `eth0` or `enpXsY` (`X` and `Y` can vary). Double check that your IPv4 address is there and take note of the naming of the ethernet. Ensure that it shows `state UP`. If it is down, use `sudo ip link set eth0 up`, replacing `eth0` with whatever its name actually was.
6. Enable IP forwarding using `sudo sysctl -w net.ipv4.ip_forward=1`. You can permanently set this by adding the line `net.ipv4.ip_forward=1` to the file `/etc/sysctl.conf`. Be sure to appl the changes using `sudo sysctl -p` once you're done.
7. You can disable your firewall to test connectivity. You can do so with `sudo ufw disable`. For security, it is good practice to reenable it when you are done sharing ethernet with `sudo ufw enable`. You can also allow forwarding of the firewall if you want it to remain active by doing `sudo ufw allow from 10.42.0.0/24`. You replace the IP address with whatever you took note of before and adding `0` to the last part of it.

## On the non-WiFi machine
1. Open a terminal and let's test the connection.
2. Ping the IPv4 address of the WiFi machine: `ping 10.42.0.1`. If we failed at this stage, check your ethernet connection and check that you're using the correct IPv4 address from the WiFi machine. You can find it as described above.
3. Ping an external IP address (e.g., Google's): `ping 8.8.8.8`. If we failed at this stage, check your routing setup for IPv4 forwarding. You may also need to set up NAT, which we can do **ON THE WIFI MACHINE** with:
```bash
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
sudo iptables -A FORWARD -i wlan0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o wlan0 -j ACCEPT
```
Replace `eth0` with the value we took note of before. Replace `wlan0` with the actual WiFi connection name from `ip a` (e.g., could be `wlpXsY`).
4. Ping an excternal domain name  (e.g., Google's): `ping google.com`. If we failed at this stage, there is a failure with DNS resolution.

At this point if it works, then you have internet on your non-WiFi device!