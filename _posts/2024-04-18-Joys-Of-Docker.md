# The Joys of Docker

Today I tried to get docker desktop up and running on my Windows 10 machine. Along the way I encountered an onslaught of errors & issues. So to save future agony, I thought that this was a great place to document my journey. Let's start with a brief introduction.

## Why use Docker?

Docker is an OS virtualisation tool which allows you to run `container` instances on your host machines. `Containers` are isolated machines which you can custom build depending on your requirements. Working in containers is ideal as it allows you to work in a consistent & reproducible environment without posing risks to the host system. If you make a mistake in a container, it can easily be disposed of and rebuilt from scratch.

For Deep Learning working in containers is favourable, as you can maintain a consistent environment and configuration for each individual project. As part of *ELEC4630: Computer Vision & Deep Learning* I was in the process of configuring Docker Desktop so I could perform DL training in WSL. This is where I came across a few errors.

## Error 1 - Device Storage

Whilst Docker enables you to work in containerised instances and do some pretty incredible things, if you are constantly trying to spin up new containers, this will chew through your available disk space. This is a lesson I learnt the hard way, where I basically ran out of storage completely and my completely and my computer was running **EXTREMELY** slow.

![Available PC Storage (Low)](../images/device%20storage.PNG "Available Disk Storage")

![Docker Disk Usage](../images/docker%20disk%20usage.PNG "Docker Disk Utility View")

As you can see I have ~40GB of storage being used by docker containers and images.

To solve this problem, you can move all of your Docker Data to another drive. As you can see in the image above I was lucky and already had an additional 1TB Data SSD installed in my machine which I will be using.

To move all your data follow the below steps:

1. Open Docker Desktop
2. Navigate to the settings & click on resources
3. Press `Browse` and change you disk image location to the location on your system where you want to store your Docker Data.
4. Pree `Apply & Restart` depending on size of your existing docker images, this migration process can take some time. If you are happy to just rebuild all of your containers from scratch, you can alternatively delete all of your existing containers before switching the folder.

**SOLVED** - As you can see my docker data is now distributed to another drive in my machine.

![Available PC Storage (High)](../images/device%20storage%20updates.PNG "PC Storage Available After Switching Docker Disk Drive")
