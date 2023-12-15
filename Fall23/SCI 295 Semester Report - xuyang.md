# SCI 295 Semester Report

## Introduction
ROS stands for robot operating system and ROS2 is the second version of ROS. ROS was created to make the process of designing and programming robots more efficient, with an operating system designed only for robots. 

The goal of ROS is to "increase the software reusability of robots." in other words "Don't reinvent the wheel." To give an example: For a car company, it might not be wise to start everything from 0, to reinvent the wheel, the engine, and the motor. Instead, what they usually do is to let other factors create them and put them back together in the end. 

Similarly, we can get various software components from the ROS (Robot Operating System) community, use them as a foundation to realize our own creative ideas and share our achievements with others. This way, everyone can stand on the shoulders of giants, moving forward together step by step. Through this incremental progress and collaboration, it allows intelligent robots to accumulate more knowledge and achieve sustained advancement in the long run.ROS was born in 2007 at Stanford University and opened source in 2010. 

### Switching from Choreograph to ROS.

Previously NAO robots were running with Choreograph, which is "Choregraphe is a multi-platform desktop application. It allows you to: Create animations and behaviors, Test them on a simulated robot, or directly on a real one, Monitor and control NAO."(Soft bank Robotic) Although Choregraphe is a simple way to use and make a robot perform certain tasks but is not the ideal way to program the robot. Choregraphe is more like an application for you to control and play with the robot but is very limited in performing complex tasks. ROS is the standard industrial tool for robot development and it gives you full access and control to the smallest part of the robot. 

## Method

Although ROS2 supports Windows and MacOS, it still works better with Ubuntu(Ubuntu is a Linux distribution, "A Linux distribution (often abbreviated as distro) is an operating system made from a software collection that includes the Linux kernel and often a package management system."(Wikipedia)) For Windows and MacOS users that want to access ROS2 in Ubuntu you'll need to download a virtual machine which will allow you to run a different operating system on you computer. 

The are various virtual machine applications you can download online, however, the one that worked the best for me is the "VMWare Work Station player." Download VMware player here: https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html. 
Download Ubuntu here: https://ubuntu.com/download/desktop. Download the ".iso" file, open VMware, and select the Installer disc image file (iso). Choose the ".iso" file you just downloaded from Ubuntu and create a new virtual machine. (Try to give VM as much disk space as you can, you might run out of space when downloading and compiling naoqi-driver later) After this follow Lisa's instructions on installing ROS2. 

## ROS2 Concept

### Work Space
A folder where you access, and organize all your code. There will be four folders there. 
- src, code you want to compile in the future
- build, intermediate files generated during the compilation process
- install, any file or code that is ready to executes
- log, during the compilation and execution process, save various logs such as warnings, errors, and information

### Package
"A package is an organizational unit for your ROS 2 code. If you want to be able to install your code or share it with others, then you’ll need it organized in a package. With packages, you can release your ROS 2 work and allow others to build and use it easily." (ROS2 Documentation Humble)

More information about ROS2 humble can be found here: https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html

## Result:

In order for ROS2 to connect with NAO robot, ROS2 has to talk to "naoqi-driver" first. "naoqi-driver" act as a bridge for them to communicate. In the early semester, I was trying to connect ROS with NAO itself without "naoqi-driver." but not with any success(might be feasible if there was more time). My hypothesis is that if we can get ROS2 to talk to NAO directly there might exist more online instruction on how ROS2 can work with humanoid robots such as NAO. In comparison to "naoqi-driver" the instructions and tutorials might be very limited.

In the end, NAO was successfully connected to ROS2 and we were able to run some simple instructions such as walking and talking.

## Conclusion:

This semester we mainly have been focusing on how to make ROS2 to work with NAO. Although facing many obstacles and frustration during the process we learned so much about ROS, NAO and the whole research experience. We know it is a difficult route to take with nearly 0 experience with ROS but we know it is the necessary step we have to take. In the future semester, we can explore ROS2 and NAO further and create a program that can manipulate robots' behavior in more depth. By switching to ROS2 we open the possibilities for more advanced sensory processing, such as computer vision, natural language processing, and full autonomy. 














references: https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html Retrieved date 12/15/23









