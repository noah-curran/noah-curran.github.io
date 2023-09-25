---
layout: page
title: AV Safety and Security Reading Group
permalink: /avsafesec/
---

Alongside [Ryan Feng](http://www-personal.umich.edu/~rtfeng/), I am co-organizing a reading group for Autonomous Vehicle (AV) Safety and Security. This is hosted in the CSE Department at the University of Michigan, but anyone who is able to make it in-person is welcome to join.

# What is AV "Safety" and "Security"?

{% include image.html url="/assets/images/avsafesec.jpg" caption="Major components to AV safety and security." width=300 align="right" %}

To guide this question, take a look at the figure. During the process of AV operation there are five general components that we observe having important safety/security concerns that the literature focuses on.

This taxonomy is not meant to be complete, but rather a guide for our reading group; there are definitely a few missing components during our simplification. If there are topics related to AV safety and security that don't fall into our definition, please introduce them during our reading group meetings!

## Environment State

The state of the environment has sensitive and private information. Since autonomous vehicles have diverse perception systems, this information is collected and stored locally/remotely. This data maybe be used to diminish upon one's ability to exist in public without being spied on.

## Sensor System

Failures and attacks may directly compromise the sensor system's ability to deliver correct data. Perception fault detection and mitigation is thus an active area of research. Downstream impact on the control of the vehicle is an important area of interest here.

## Compute Environment

Autonomous vehicle middleware, such as ROS or ArduPilot, facilitates the communication and management of important sensor data and control choices. If the middleware is compromised or incorrect, this can have dangerous effects on the AV's control.

## Algorithm

AVs have a few types of control algorithms. Some have bounded known error that they can have given a set of assumptions, but faulty sensor data may cause violations of these assumptions. Likewise, deep neural networks may have distribution shifts due to various environmental factors. These issues with the algorithms may cause control failure.

## Environment Interaction

There are physical constraints that the AV must operation within as it traverses its environment (e.g., hard deadlines, social patterns, reacting to context). An adversary or fault can cause the AV to violate the constraints of the environment and cause dangerous outcomes.

# Goals and Expectations

1. This reading group will focus more on the discussion of the chosen papers, so we expect each attendee to read/skim the paper enough to have a basic understanding.

2. Each week a volunteer will lead the discussion for a paper of their choosing. The volunteer may use some pre-existing slide deck from a conference, but no expectation to make their own slides. The volunteer may give a brief (~15 min.) summary of the paper, but the majority of the time should be for open discussion.

3. During the last few minutes, the volunteer will work with the group to summarize the takeaways from the discussion. The idea is to have something to "chew on" mentally after we leave the meeting. To make this easier, one participant may volunteer to take notes during the meeting so that we can share them with the group (and with permission we may share them publicly if we find them to be a useful resource).

4. Finally, the primary goal of this reading group is to gain exposure to a wide range of topics related to AV safety and security. If an interesting paper is not strictly covered by our above definition yet seems related, we encourage the volunteer to introduce the paper. It may be beneficial to everyone to see something outside of the norm.

# When? Where? Email List?

Fridays from 12:30 PM to 1:30 PM in the CSE Grad Lounge (until we can get a real conference room booked).

[Join the MCommunity group here to receive emails.](https://mcommunity.umich.edu/group/AV%20Safety%20Reading%20Group)

<!-- ### How Can I Volunteer?

Come to one of our meetings -->

# Upcoming Schedule

{% include readinggroupevent.html date="Oct. 6th" volunteer="Minkyoung Cho" title="TBD" url="" %}
{% include readinggroupevent.html date="Sept. 29th" volunteer="Liangkai Liu" authors="Ionel Gog, et al" title="D3: A Dynamic Deadline-Driven Approach for Building Autonomous Vehicles" pubvenue="in the Proceedings of the Seventeenth European Conference on Computer Systems (EuroSys '22)" year="2022" url="https://dl.acm.org/doi/10.1145/3492321.3519576" supp="https://arxiv.org/pdf/2302.01568.pdf" %}

# Paper List

*Some papers may fit into multiple categories. We place them in the main area we feel their contributions focus on.*

#### &#9312; Environmental Privacy

{% include readinggrouppaper.html authors="Chulin Xie, et al" title="Privacy of Autonomous Vehicles: Risks, Protection Methods, and Future Directions" pubvenue="arXiv:2209.04022" year="2022" url="https://arxiv.org/abs/2209.04022" %}

{% include readinggrouppaper.html authors="Dorothy J. Glancy" title="Privacy in Autonomous Vehicles" pubvenue="Santa Clara Law Review" year="2012" url="https://digitalcommons.law.scu.edu/lawreview/vol52/iss4/3/" %}

{% include readinggrouppaper.html authors="Luis F. Alvarez León" title="Eyes on the Road: Surveillance Logics in the Autonomous Vehicle Economy" pubvenue="Surveillance & Society" year="2019" url="https://ojs.library.queensu.ca/index.php/surveillance-and-society/article/view/12932" %}

#### &#9313; Sensor System Attacks/Failures

{% include readinggrouppaper.html authors="Yulong Cao, et al" title="You Can't See Me: Physical Removal Attacks on LiDAR-based Autonomous Vehicles Driving Frameworks" pubvenue="in the Proceedings of the 32nd USENIX Security Symposium (Security '23)" year="2023" url="https://www.usenix.org/conference/usenixsecurity23/presentation/cao" %}

{% include readinggrouppaper.html authors="Qifan Xiao, et al" title="Exorcising 'Wraith': Protecting LiDAR-based Object Detector in Automated Driving System from Appearing Attacks" pubvenue="in the Proceedings of the 32nd USENIX Security Symposium (Security '23)" year="2023" url="https://www.usenix.org/conference/usenixsecurity23/presentation/xiao-qifan" %}

#### &#9314; Computing Environment Integrity

{% include readinggrouppaper.html authors="Hyungsub Kim, et al" title="PGFUZZ: Policy-Guided Fuzzing for Robotic Vehicles" pubvenue="in the Proceedings of the Network and Distributed Systems Security Symposium 2021 (NDSS '21)" year="2021" url="https://www.ndss-symposium.org/wp-content/uploads/ndss2021_6A-1_24096_paper.pdf" %}

{% include readinggrouppaper.html authors="Yunpeng Luo, et al" title="Poster: Towards Complete Computation Graph Generation for Security Assessment of ROS Applications" pubvenue="in the Proceedings of the 2022 ACM SIGSAC Conference on Computer and Communications Security (CCS '22)" year="2022" url="https://dl.acm.org/doi/abs/10.1145/3548606.3563540" %}

{% include readinggrouppaper.html authors="Bernhard Dieber, et al" title="Application-Level Security for ROS-Based Applications" pubvenue="in the Proceedings of the 2016 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS '16)" year="2016" url="https://ieeexplore.ieee.org/abstract/document/7759659" %}

{% include readinggrouppaper.html authors="R. Spencer Hallyburton, et al" title="Partial-Information, Longitudinal Cyber Attacks on LiDAR in Autonomous Vehicles" pubvenue="arXiv:2303.03470" year="2023" url="https://arxiv.org/abs/2303.03470" %}

#### &#9315; Robustness of Control/Decision Algorithms

{% include readinggrouppaper.html authors="Maximilian Jaritz, et al" title="xMUDA: Cross-Modal Unsupervised Domain Adaptation for 3D Semantic Segmentation" pubvenue="in the Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR '20)" year="2020" url="https://openaccess.thecvf.com/content_CVPR_2020/html/Jaritz_xMUDA_Cross-Modal_Unsupervised_Domain_Adaptation_for_3D_Semantic_Segmentation_CVPR_2020_paper.html" %}

{% include readinggrouppaper.html authors="Abhinav Valada, et al" title="AdapNet: Adaptive Semantic Segmentation in Adverse Environmental Conditions" pubvenue="in the Proceedings of the 2017 IEEE International Conference on Robotics and Automation (ICRA '17)" year="2017" url="https://ieeexplore.ieee.org/abstract/document/7989540" %}

{% include readinggrouppaper.html authors="Oier Mees, et al" title="Choosing Smartly: Adaptive Multimodal Fusion for Object Detection in Changing Environments" pubvenue="in the Proceedings of the 2016 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS '16)" year="2016" url="https://ieeexplore.ieee.org/abstract/document/7759048" %}

{% include readinggrouppaper.html authors="Jinlong Li, et al" title="Domain Adaptive Object Detection for Autonomous Driving Under Foggy Weather" pubvenue="in the Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision (WACV '23)" year="2023" url="https://arxiv.org/abs/2210.15176" %}

{% include readinggrouppaper.html authors="Dogancan Temel, et al" title="CURE-TSR: Challenging Unreal and Real Environments for Traffic Sign Recognition" pubvenue="Neural Information Processing Systems (NeurIPS) Workshop on Machine Learning for Intelligent Transportation Systems (MLITS)" year="2017" url="https://arxiv.org/abs/1712.02463" %}

{% include readinggrouppaper.html authors="Sabbir Ahmed, et al" title="DFR-TSD: A Deep Learning Based Framework for Robust Traffic Sign Detection Under Challenging Weather Conditions" pubvenue="IEEE Transactions on Intelligent Transportation Systems" year="2022" url="https://ieeexplore.ieee.org/abstract/document/9345465" %}

#### &#9316; Environmental Constraints/Impact

{% include readinggrouppaper.html authors="Yanmao Man, et al" title="That Person Moves Like A Car: Misclassification Attack Detection for Autonomous Systems Using Spatiotemporal Consistency" pubvenue="in the Proceedings of the 32nd USENIX Security Symposium (Security '23)" year="2023" url="https://www.usenix.org/conference/usenixsecurity23/presentation/man" %}
