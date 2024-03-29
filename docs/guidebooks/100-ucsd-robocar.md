# UCSD Robocar Framework

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

## UCSD Robocar Framework Breakdown

Below are the supporting packages to the framework. The Nav package operates as the "brain" because it is the only package that communicates to all the other packages which are all independent from one another.
​
Why so many packages? In practice, developing stand-alone or independent functionalities makes the package more robust in terms of deployability. Also as the robot becomes more sophisticated, the number of packages it will have access to would naturally increase allowing it to achieve many different types of tasks depending on the application of interest.

So the idea is to develop a package that could in general be used on any car-like robot as well as being able to choose what packages your robot really needs without having to use the entire framework. 
​
For example, lets say another company developed their own similar sensor, actuator and nav packages but they have not researched into lane detection. Instead of using the entire UCSD Robocar framework, they could easily just deploy the lane detection package and have some interpreter in their framework read the messages from the lane detection package to suit their needs.

Link to official GitLab repo (**ROS**): [ucsd_robocar_hub1](https://gitlab.com/ucsd_robocar/ucsd_robocar_hub1)  
Link to official GitLab repo (**ROS2**): [ucsd_robocar_hub2](https://gitlab.com/ucsd_robocar2/ucsd_robocar_hub2)

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

### Updating all packages

A utility function was added to the `~/.bashrc` script that will automatically update all the packages in the framework and then rebuild and source it so it will be ready to start using ROS2!

From a terminal, run:

```sh
upd_ucsd_robocar
```

### Launch Files

The launch file diagrams below show the very general approach of how the packages communicate with one another. With ROS, it just comes down to a combination of starting launch files and sending messages (through topics) to nodes. For specific details about messages types, topics, services and launch files used, please go to the readme for the specific package of interest!

The nav_pkg is at the base of each of the diagrams and rooting from it are the launch files it calls that will launch other nodes/launch files from all the other packages in the framework.

In ROS2, a dynamically built launch file (at run-time) is used to launch all the different nodes/launch files for various purposes such as data collection, navigation algorithms and controllers. 

This new way of creating launch files has now been simplified by just adding an entry to a `yaml` file of where the launch file is and a separate `yaml` file to indicate to use that launch file or not. 

There is only one file to modify, and all that needs to be changed is either putting a `0` or a `1` next to the list of nodes/launch files. To select the nodes that you want to use, put a `1` next to it otherwise put a `0` which means it will not activate. 

In the figures below, instead of including the entire `ros2 launch` command, you will only see the names of the launch files that need to be turned on in the node config file explained more in detail [here](https://docs.google.com/document/d/1YS5YGbo8evIo9Mlb0J-w2r3bZfju37Zl4UmdaN2CD2A/edit#heading=h.3ucils4zejvp).

*TODO: images of node mapping*

## Developer Tools

### ROS Guidebooks

Links provided below are guides for ROS and ROS2 which include many examples, terminal commands and general concept explanations of the various features in ROS and ROS2.

- [UCSD ROS Guidebook](https://docs.google.com/document/d/1u7XS7B-Rl_emK3kVKEfc0MxHtwXGYHf5HfLlnX8Ydiw/edit)
- [UCSD ROS2 Guidebook](https://docs.google.com/document/d/1DJgVLnu_vN-IXKD3QrQVF3W-JC6RiQPVugHeFAioB58/edit?usp=sharing)

### GitLab

Since the framework uses a *meta package* (a package that contains multiple packages) we refer to individual packages as **submodules**.

#### Adding new submodules

```sh
git submodule add <remote_url>
git commit -m "message"
git push
```

#### Updating local submodules with remote submodules

If local changes have been made, the update command will fail unless you add, commit and push (shown in the following subsection) or stash (`git stash`) them, which will temporarily discard any local changes.

```sh
git submodule update --remote --merge
```

*Pay attention to the output of this command to make sure it did not fail or abort.*

#### Updating remote submodules with local submodules

```sh
git add .
git commit -m "message"
git push
```

Again, *pay attention to the output*.

#### Removing submodules

```sh
git submodule deinit <submodule>
git rm <submodule>
```

#### Adding an existing package to git

From the web browser, [create an empty repo on GitLab](https://docs.gitlab.com/ee/user/project/working_with_projects.html#create-a-blank-project).

Now from the Jetson, start by creating a new ROS2 package.

```sh
ros2 pkg create --build-type ament_python pkg_name --dependencies rclpy
build_ros2
```

Now proceed with merging the new package into the framework.

```sh
git init
git remote add origin <remote-url-from-step-one>
git add .
git commit -m "message"
git push --set-upstream origin master
```

### Docker Commands

Below is a go-to list of docker commands that can be used with the framework.

Some new lingo:  
Container name = `NAME`  
Image name = `REPOSITORY`  
Image tag ID (comparable to branches in git) = `TAG`

#### Pulling/running

To pull an image from Docker Hub:

```sh
docker pull REPOSITORY:TAG
```

Starting a stopped container:

```sh
docker start NAME
```

Stopping a running container:

```sh
docker stop NAME
```

To open a new terminal for a Docker container that is currently running:

```sh
docker exec -it NAME bash
```

Building Docker image by giving it a new name and tag:

```sh
docker build -t REPOSITORY:TAG .
```

#### Updating/creating/sharing

To save changes made while in container to the original image, change the tag to create a new image including your changes:

```sh
docker commit NAME REPOSITORY:TAG
```

Creating a new image from a container:

```sh
docker tag NAME REPOSITORY:TAG
```

Pushing image to Docker Hub:

```sh
docker push REPOSITORY:TAG
```

To share files between host and docker container:

- From **host** to container:

```sh
docker cp <filename>.txt <container-ID>:/<filename>.txt
```

- From **container** to host:

```sh
docker cp <container-ID>:/<filename>.txt <filename>.txt
```

!!! note
    Both of these commands need to be run from a **host terminal**, they won't work if you're inside the Docker container.

#### Listing

To list all images:

```sh
docker images
```

To list all running containers:

```sh
docker ps
```

To list all containers including those not running:

```sh
docker ps -a
```

#### Deleting

To delete a specific container:

```sh
docker rm NAME
```

To delete a specific image:

```sh
docker rmi REPOSITORY:TAG
```

To delete ALL containers:

```sh
docker rm -f $(docker ps -a -q)
```

To delete ALL images:

```sh
docker rmi -f $(docker images -q)
```

## Accessing Docker images

Currently there are two DIFFERENT docker images that are being supported by UCSD. One image was built for arm architecture computers (Jetson family) and the other was built for X86 architecture computers (most laptops and desktops). Apple M1 support will be coming soon.

**Question:** Why two images?

**Answer:** The X86 image was built to provide an environment for the developer to test new algorithms, packages, sensors (Yes, you can plug sensors into your computer just like the Jetson for testing) etc in a simulated environment without having to use a physical robot. Using the physical robot for first-time testing can lead to damaging the robot or something/someone in the environment due to an unforeseen behavior from the robot. We must practice safe autonomy if we ever hope to see our new ideas become a part of the industry! This leads to the ARM image, which was built to be used on the physical robot when ready to perform physical testing.

**Question:** The display wont open when in the container, how to make it work? (ie. images won't port through)

**Answer:** There could be several reasons why the display is not working but below are the most common solutions that can be tried:

- Make sure that an [X11 forwarding session](#enable-x11-port-forwarding) was established when running SSH connection into the JTN.
- If that still does not work, the container could have a broken connection with the display. If this is the case, try [creating a new container](#running-a-container-on-the-jtn) using the provided function in the `~/.bashrc` script.

!!! note
    Docker is pre-installed on the Jetson computers so no need to install it, but in order to use the X86 image, you must install Docker on your computer.

### UCSD Robocar Image

The image is hosted on Docker Hub [here](https://hub.docker.com/r/djnighti/ucsd_robocar).

- Computer architechture `ARM` (Jetson)  
To pull the image from the terminal:

```sh
docker pull djnighti/ucsd_robocar:latest
```

- Computer architecture `X86` (PC)  
To pull the image from the terminal:

```sh
docker pull djnighti/ucsd_robocar:x86
```

### Docker Setup

The exact "recipe" to build this image can be found [here](https://gitlab.com/ucsd_robocar2/ucsd_robocar_hub2/-/blob/master/docker_setup/docker_files/Dockerfile).

!!! note
    If you are using the virtual machine, all of this is already completed for you!

#### Enable X11 Port Forwarding

From your *host machine* **(not the Jetson)** enter these commands. You will have to enter this in your terminal **every time** before you can SSH into the JTN.

```sh
xhost +
ssh -X jetson@your-ip-address
```

Now, from inside the JTN, run the following commands to obtain `sudo` privileges for docker commands. This only needs to be run one time.

```sh
sudo usermod -aG docker $(USER)
su $(USER)
```

Now, to see if everything is working correctly, test your X11 forwarding with:

```sh
xeyes
```

If you see some googly eyes pop up on your screen, X11 is ready to go. 

If X11 port forwarding is **not set up** on your machine, follow the steps [here](https://gitlab.com/djnighti/ucsd_robo_car_simple_ros/-/blob/master/x11_forwarding_steps.txt) to get it started. Then return to these steps and try them again.


#### Update Docker daemon

To ensure Docker containers can be run properly on the JTN, modify the `daemon.json` file by deleting the previous version and creating a new one.

```sh
sudo rm /etc/docker/daemon.json
sudo nano /etc/docker/daemon.json
```

Now copy and paste the following into the file you just created.

```
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    },
    "default-runtime": "nvidia"
}
```

Save, quit, and reboot the JTN for your changes to be applied.

```sh
sudo reboot
```

#### Running a container on the JTN

SSH back into the JTN after you reboot, with the `-X` flag indicating X11 forwarding is enabled.

```sh
ssh -X jetson@your-ip-address
```

We can now create a shortcut to easily run a container with all the necessary arguments from the command line by adding a function to the `~/.bashrc` file.

```sh
gedit ~/.bashrc
```

At the very bottom of the file, paste the following.

```
robocar_docker ()
{
    docker run \
    --name ${1}\
    -it \
    --privileged \
    --net=host \
    -e DISPLAY=$DISPLAY \
    -v /dev/bus/usb:/dev/bus/usb \
    --device-cgroup-rule='c 189:* rmw' \
    --device /dev/video0 \
    --volume="$HOME/.Xauthority:/root/.Xauthority:rw" \
    djnighti/ucsd_robocar:${2:-devel}
}
```

Notice the 2 arguments we have made:

- `${1}`: This will be the name of the container, i.e. `test_container`
- `${2:devel}`: This is the tag ID of the image you want to launch a container from. If nothing is specified when calling at the command line (example shown below), the `devel` tag will be run automatically.

<u><b>Don't modify the function. The arguments are intentional and not meant to be hard-coded.</u></b>


Once you've added the lines to your `~/.bashrc` and saved the file, source the script so your current terminal can see the new function you just added. Then, run the command to enter a new container.

```sh
source ~/.bashrc
robocar_docker test_container
```

This will start a container called `test_container` built from the image with default tag ID `devel`.

To access this same container from another terminal:

```sh
docker exec -it test_container bash
```

At this point, your Docker setup is complete, but don't forget to refer to the useful [Docker commands](#docker-commands) which include deleting, creating and updating images locally and remotely!

