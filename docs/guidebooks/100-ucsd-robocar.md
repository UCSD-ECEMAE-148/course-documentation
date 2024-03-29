# Introduction

The UCSD Robocar framework is primarily maintained and developed by Dominic Nightingale right here at UC San Diego.

UCSD Robocar uses ROS and ROS2 for controlling our scaled robot cars which can vary from traditional programming or machine learning to achieve an objective. The framework works with a vast selection of sensors and actuation methods in our inventory making it a robust framework to use across various platforms. Has been tested on 1/16, 1/10, 1/5 scaled robot cars and soon our go-karts.

## About

This framework was originally developed as one of Dominic’s senior capstone projects as an undergraduate and has been under constant development throughout his graduate program. The framework provides the ability to easily control a car-like robot as well as performing autonomous tasks. It is currently being used to support his thesis in learning-model predictive control (LMPC).

The framework is also being used to teach undergraduates the fundamentals of using gitlab, docker, python, openCV and ROS. The students are given the task to use the framework with their robots to perform autonomous laps on a track by first going through a calibration process that's embedded into the framework. The students then have to come up with their own final projects for the class that can be supported by the framework, which can vary from car following, SLAM applications, path planning, city driving behaviors, Human-machine-interfacing and so much more.

## What's Being Used

### Embedded Computers

There are 3 main computers that have been used to develop and test this framework which belong to the NVIDIA Jetson family. 

- Jetson Nano
- Jetson Xavier Nx
- Jetson AGX Xavier

### Ubuntu

The host OS on all the Jetson computers use Ubuntu18 which is flashed through NVIDIA's Jetpack image. However, the docker image uses Ubuntu20 in order to use ROS2 without worrying about package installation issues.

### GitLab

This is where all the code for the entire framework is managed and developed. Gitlab provides a service similar to google drive but for programs! It's especially convenient in terms of deploying code into embedded computers.

### Docker

This tool is being used to expedite the setup process on the computers. To get the docker image working, the Jetson just needs to be flashed with the Jetpack 4.6 image provided by NVIDIA and then simply pull the UCSD Robocar docker image from docker hub onto the Jetson. This allows for plug-n-play capabilities as long as all the hardware is connected to the Jetson properly.

### ROS/ROS2

The framework allows for both ROS-Noetic and ROS2-Foxy to work together through the ROS bridge or independently depending on the application.

## Recommendations

### VS Code IDE

Microsoft Visual Studio IDE is an excellent development tool for coding especially because of all the free plug-ins that can be added.

Plug-ins recommended:

- Python
- Docker
- Remote SSH

### Virtual Machines

If having software related issues, a virtual machine can possibly solve the issues and also provide a linux based interface to use with the jetson which is usually much smoother than with Windows or Mac.

For details on how to install Virtual Machine software, see the [Virtual Machine Guide](../guidebooks/50-vm-guide.md) section of this website.

To download a virtual machine image that runs Ubuntu20.04, has VS code (with all plug-ins mentioned above), Docker and the UCSDrobocar docker image installed already, click [here](https://drive.google.com/file/d/1ltKrZBdA2ZTFRjKj5E08WWbZN5PNIPJL/view?usp=sharing).

Once you've downloaded the image and set it up with VMware, use the following credentials for login.

Hostname: `ucsdrobocar-vm`  
Username: `robocar`  
Password: `ucsdrobocar`  

# UCSD Robocar Framework Breakdown

Below are the supporting packages to the framework. The Nav package operates as the "brain" because it is the only package that communicates to all the other packages which are all independent from one another.
​
Why so many packages? In practice, developing stand-alone or independent functionalities makes the package more robust in terms of deployability. Also as the robot becomes more sophisticated, the number of packages it will have access to would naturally increase allowing it to achieve many different types of tasks depending on the application of interest.

So the idea is to develop a package that could in general be used on any car-like robot as well as being able to choose what packages your robot really needs without having to use the entire framework. 
​
For example, lets say another company developed their own similar sensor, actuator and nav packages but they have not researched into lane detection. Instead of using the entire UCSD Robocar framework, they could easily just deploy the lane detection package and have some interpreter in their framework read the messages from the lane detection package to suit their needs.

Link to official GitLab repo (**ROS**): [ucsd_robocar_hub1](https://gitlab.com/ucsd_robocar/ucsd_robocar_hub1)  
Link to official GitLab repo (**ROS2**): [https://gitlab.com/ucsd_robocar2/ucsd_robocar_hub2](https://gitlab.com/ucsd_robocar2/ucsd_robocar_hub2)

!!! note
  Both `hub1` and `hub2` are **metapackages**. For specific details about any individual package, click on any of the packages in either hub to be taken to that packages' main repository.

## Packages

**Each UCSD ROS package has a README.md that explains in detail what config, nodes, launch files it has as well as topic/message information. So if you are confused about a particular thing, ask yourself: “What is the problem I am having?” and “What package is most likely the root of the concern?” Then go see the readme for that package and check anything relevant or even the troubleshooting section.**

In the package sections below are the links to the official README.md docs for each package for both ROS1 and ROS2. So any package with a 1 in it is for ROS-NOETIC and any package with a 2 is for ROS2-FOXY.

### Nav

The navigation package (nav_pkg) is the "brain" of the UCSD Robocar framework because it keeps all the launch files in its package to launch any node/launch file from the other packages used in the framework. This makes using the framework easier because you only really have to remember the name of the nav_pkg and what launch file you want to use rather than having to remember all the other package names and their own unique launch files.

Resources:  
[`NAV2_README.md`](https://gitlab.com/ucsd_robocar2/ucsd_robocar_nav2_pkg/-/blob/master/README.md)  
[`NAV1_README.md`](https://gitlab.com/ucsd_robocar/ucsd_robocar_nav1_pkg/-/blob/master/README.md)

### Lane Detection

The lane detection package is one method of navigating by identifying and tracking road markers. The basic principle behind this package is to detect road markers using openCV and then compute whats called the “cross-track-error” which is the difference between the center axis of the car and the centroid (center of “mass”) of the road mark which is then fed into a PID controller for tracking.

Resources:  
[`Lane_Detection2_README.md`](https://gitlab.com/ucsd_robocar2/ucsd_robocar_lane_detection2_pkg/-/blob/master/README.md)  
[`Lane_Detection1_README.md`](https://gitlab.com/ucsd_robocar/ucsd_robocar_lane_detection1_pkg/-/blob/master/README.md)

### Sensor

The sensor package contains all the required nodes/launch files needed to use the sensors that are equipped to the car.
[`Sensor2 README.md`](https://gitlab.com/ucsd_robocar2/ucsd_robocar_sensor2_pkg/-/blob/master/README.md)
[`Sensor1 README.md`](https://gitlab.com/ucsd_robocar/ucsd_robocar_sensor1_pkg)

### Actuator

The actuator package contains all the required nodes/launch files needed to use the actuators that are equipped to the car.  
[`Actuator2 README.md`](https://gitlab.com/ucsd_robocar2/ucsd_robocar_actuator2_pkg/-/blob/master/README.md)
[`Actuator1 README.md`](https://gitlab.com/ucsd_robocar/ucsd_robocar_actuator1_pkg/-/blob/master/README.md)

### Control (coming soon)

The control package contains all the required nodes/launch files needed to control the car in various methods such as PID, LQR, LQG and MPC.

### Path (coming soon)

The path package contains all the required nodes/launch files needed to create trajectories for the car to follow in a pre-built map as well as in simulations 

### Basics

The path package contains all the required nodes/launch files needed to subscribe/publish to the sensor/actuator messages within the framework for fast algorithm prototyping.

[`Basics2 README.md`](https://gitlab.com/ucsd_robocar2/ucsd_robocar_basics2_pkg/-/blob/master/README.md)

## Updating all packages

A utility function was added to the ~/.bashrc script that will automatically update all the packages in the framework and then rebuild and source it so it will be ready to start using ROS2!

From a terminal, run:

```sh
upd_ucsd_robocar
```





