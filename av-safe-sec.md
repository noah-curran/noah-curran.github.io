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

Join the MCommunity group here to receive emails: <https://mcommunity.umich.edu/group/AV%20Safety%20Reading%20Group>

<!-- ### How Can I Volunteer?

Come to one of our meetings -->

# Upcoming Schedule

{% include readinggroupevent.html date="Oct. 6th" volunteer="Minkyoung Cho" title="TBD" url="" %}
{% include readinggroupevent.html date="Sept. 29th" volunteer="Liangkai Liu" authors="Ionel Gog, et al" title="D3: A Dynamic Deadline-Driven Approach for Building Autonomous Vehicles" pubvenue="in the Proceedings of the Seventeenth European Conference on Computer Systems (EuroSys '22)" year="2022" url="https://dl.acm.org/doi/10.1145/3492321.3519576" supp="https://arxiv.org/pdf/2302.01568.pdf" %}

# Paper List

#### &#9312; Environmental Privacy

{% include readinggrouppaper.html authors="Chulin Xie, Zhong Cao, Yunhui Long, Diange Yang, Ding Zhao, and Bo Li" title="Privacy of Autonomous Vehicles: Risks, Protection Methods, and Future Directions" pubvenue="arXiv:2209.04022" year="2022" url="https://arxiv.org/abs/2209.04022" supp="" %}

{% include readinggrouppaper.html authors="Dorothy J. Glancy" title="Privacy in Autonomous Vehicles" pubvenue="Santa Clara Law Review" year="2012" url="" supp="" %}

{% include readinggrouppaper.html authors="Luis F. Alvarez León" title="Eyes on the Road: Surveillance Logics in the Autonomous Vehicle Economy" pubvenue="Surveillance & Society" year="2019" url="https://ojs.library.queensu.ca/index.php/surveillance-and-society/article/view/12932" supp="" %}

{% include readinggrouppaper.html authors="" title="" pubvenue="" year="" url="" supp="" %}

#### &#9313; Sensor System Attacks/Failures

{% include readinggrouppaper.html authors="" title="" pubvenue="" year="" url="" supp="" %}

#### &#9314; Computing Environment Integrity

#### &#9315; Robustness of Control/Decision Algorithms

#### &#9316; Environmental Constraints/Impact
