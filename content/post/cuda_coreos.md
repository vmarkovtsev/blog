---
author: mcuadros
date: 2016-12-03
title: "NVIDIA CUDA + CoreOS = the best environment for deep learning."
draft: false
image: /post/cuda_coreos/intro.png
description: "GPGPU computing environment can be set up very neat inside a Docker container running on CoreOS. Our way of organizing deep learning on premises is awesome and brings many benefits for devops and data scientists."
---

We've had a discussion about the best way of running NVIDIA CUDA applications recently.
There are two options basically:

1. Use a cloud service (Google, Amazon, Azure).
2. Buy our own hardware and maintain it.

Every option has it's own pros and cons. Clouds simplify devops, are easier
to monitor and more flexible. Your own hardware, including GPU cards,
is more expensive at the beginning, but quickly pays off - according to our
calculations, within a year period. Of course, we are not the first folks
who had to make this choice. There is an excellent presentation by Petteri
Teikari ([SlideShare](http://www.slideshare.net/PetteriTeikariPhD/deep-learning-workstation)),
it has 140 pages and convincingly states that (2) is the better choice. We quite
agree with his arguments, but the traditional software setup
([Ubuntu Linux](https://www.ubuntu.com/) on bare metal) is a nightmare to maintain.

We are running our data retrieval pipeline as a set of microservices inside Docker
containers. It used to be different a year ago with all that unstable
configuration management, but Docker really revolutionized the way we deploy
and test projects. I am a big CoreOS fan, visited quite a few conferences (and talked?)
devoted to it and containers in general. My idea was essentially to try to
organize deep learning and other NVIDIA CUDA dependent applications into isolated
Docker containers on top of CoreOS. Thus we achieve:

1. 100% reproducible boilerplate environment for data scientists.
2. Fully transparent administration: everything is inside Dockerfile-s in git.
3. Painless transition to an other machine in future, just like in a cloud.

We started with buying the best machine for deep learning we could afford,
that is, not [DGX-1](http://www.nvidia.com/object/deep-learning-system.html):

![the baby's photo](...)
<sub>*Eiso, our CEO, calls it "the baby"*</sub>

Here are the tech specs:

* Motherboard: SuperMicro ???
* 256 GB RAM
* [NVIDIA Titan X Pascal]() x2
* (0.5 + 1)TB SSD
* UPS ???

The fully mounted box weights **a lot**. Besides, when it starts, everybody
nearby thinks the office is taking off the ground.

Next, I installed CoreOS. I used a live CD with some random Linux distribution to boot
and followed the [official manual](https://coreos.com/os/docs/latest/installing-to-disk.html).
It was tedious to copy and paste long commands to the terminal before setting up
user accounts and consequently not having the ability to connect with SSH. How did you
