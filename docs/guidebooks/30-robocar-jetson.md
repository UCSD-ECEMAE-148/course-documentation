# UCSD Robocar Jetson Nano Configuration

## Introduction

The NVIDIA Jetson Nano in this document is also referred as JTN.

To learn more about the JTN, you can check out the NVIDIA documentation sources [Getting Started](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit), [Jetson Download Center](https://developer.nvidia.com/embedded/downloads), and the documentation for usage on [DonkeyCar](http://docs.donkeycar.com).

!!! info "
    In general for this course if you are using a computer with a Linux distribution like Ubuntu you will have an easier time installing the necessary Artificial Intelligence framework. Linux uses a file format that is not easily read in an Apple MacOS or MS Windows computer.  If you need to modify files in the $\mu$SD card formatted for Linux, e.g., a disk partition that uses Ext4 format, you should ask a colleague first if they have a Linux PC, then Tutors or TA for help.

Every Team is expected to try installing the necessary software based on these instructions using the $\mu$SD image, not the recovery image.  And every member of the team needs to participate during the lab embedded linux hands-on exercise. The same SBC (JTN) can be used at the class by several users without the need to connect a monitor and a keyboard to it. It is part of your learning in the course to get familiar with Embedded Linux, Head-Less Single Board Computers (SBC) like the JTN, terminal running in a PC, SSH, and remotely installing software in a SBC.

If you run out of time, based on instructor’s set deliverables, or your $\mu$SD card gets corrupted, you can use the recovery $\mu$SD card image that has all the software pre-installed. On the [10-UCSD Robocar ECE & MAE 148](https://docs.google.com/document/d/1Gy4CEKjqXZub0rz-YmxFOakj9O_ZjMmlUYe7NLdM4FY/edit#) document there is a link to a recovery $\mu$SD card image.

## Flashing microSD card for the Jetson Nano

From your PC let's prepare (flash) the microSD card ($\mu$SD card). Make sure your computer can access the Internet. If you are using one of our WiFi Access Points in one of the labs, or at one of the tracks, the first device that connects to the WiFi Access point and try to access the Internet, will need to accept the UCSD Wireless Visitor Agreement, just like when you are connecting to UCSD’s Visitor WiFi. In fact you are basically connecting to the Internet via the UCSD’s Visitor WiFi. 

### Software Installation

Etcher can use Zipped files, you don’t need to Unzip the image file if you are using Etcher. Ignore if your Windows or Mac computer tells you it can not ready the $\mu$SD card. The $\mu$SD card will be used on the JTN running Linux. It uses a file format that Windows and Mac don’t know about unless you use specialized software to do that. You don’t need it. If you are using Linux or MacOS command lines to write the disk image to a $\mu$SD card, you may need to extract the file first.

**Download the UCSD Jetson Nano Developer Kit $\mu$SD Card Image [here](https://drive.google.com/file/d/1Ikdat_BaP3IqEYoYfjfMQJGgmAC-UGNK/view?usp=sharing).**

Once you have the image, install Etcher to be able to write the disk image to a microSD card [here](https://www.balena.io/etcher/).

Using the image we are providing as a starting point has some advantages with some pre-configuration. For example, it won't require a monitor and a keyboard connected to the JTN to get you started.

## Writing an image to a $\mu$SD card

Connect a $\mu$SD adapter to your PC.

1. Insert the provided $\mu$SD card (64 gigabytes) into the $\mu$SD adapter.
2. Install and run Etcher.
3. Start Etcher, choose the Zipped file with the Disk Image you downloaded, pay attention when choosing the drive with the $\mu$SD card on it (e.g., 64 gigabytes).
4. Write the image to $\mu$SD card.
5. Eject the $\mu$SD card from your PC first using the procedure for your PC Operating System (e.g., eject drive) 
6. Then insert the $\mu$SD card into the JTN $\mu$SD card slot. Note this is a push-in-to-lock and push-in-to-unlock the $\mu$SD card. Please do not pull the $\mu$SD card out of the slot before unlocking it, otherwise you may damage your JTN and or $\mu$SD card.

## Powering on the JTN

The preferable way to power the JTN is with the provided 5V 4A power supply; it has a barrel connector that plugs into the JTN. You need to place a jumper connector at J48, there is a text label close to it that says
ADD JUMPER TO DISABLE USB PWR. 

For the initial configuration you don’t have to use the 5V 4A power supply. If you are not using the provided power supply for the initial configuration, you need to remove the jumper located above the JNT power connectors to allow it to be powered by the uUSB cable. Please save the jumper. You can leave it in place connected to one of the pins of J48.

Power on the JTN by connecting the power supply to a power outlet and the barrel jack into the barrel port on the Jetson. 

!!! note
    Give the Jetson a minute or two to boot and load its software and the fan may not work until you install the software later in these instructions.

## Wired communication with the JTN

We will use the command line to access the Jetson Nano (JTN) using a secure shell (SSH). Don’t worry, if you are not familiar with terminals and using command lines, you will master it in this class. Mastering these will be a good skill to have and another mention in your resume.

The Linux OS running on the JTN enables two protocols of communication: Serial Communication and SSH. Both use a local network interface with TCP/IP over the USB cable. By using a micro USB cable between the JTN and the PC, you can communicate with the JTN. On the PC, you need to use software to enable serial communication or SSH. e.g., Serial Communication - screen on Linux or MacOS. CoolTerm is another option that runs on Linux, MacOS, and MS Windows. SSH is natively supported on Linux and MacOS. MS Windows 10 or later supports SSH too.

!!! note
    Your JTN should have a WiFi / Bluetooth network interface and antennas already installed on it. If not, please contact the Tutors or TAs.

### SSH Communication (via USB connection)

Connect a $\mu$USB cable between the JTN and your PC. Then, from a terminal, run:

```sh
ssh jetson@192.168.55.1
```

Or:

```sh
ssh jetson@ucsdrobocar-xxx-yy.local
```

The default UserID and password is:

Username: `jetson` <br>
Password: `jetsonucsd`

### Serial Communication

If needed, install a software called `screen` on your host or virtual Linux computer and/or update it.

```sh
sudo apt-get install screen
sudo apt-get update screen
```

Or you can install coolterm, or use any terminal emulator software you have. On my computer the JTN plugged in using the uUSB cable shows up as `/dev/ttyACM1`. Log-in info when doing a serial connection is the same as the SSH connection

The command line to connect in a Linux terminal is:

```sh
screen /dev/ttyACM1
```

## Wireless communication with the JTN

After connecting to the JTN via USB connection, let's configure the JTN to connect to a WiFi Access Point. Using SSH via USB connection or using the serial communication software log into the JTN.

First, let's make sure the network service is running.

```sh
sudo systemctl start networking.service
```

### Accessing WiFi networks

To list the available WiFi networks:

```sh
sudo nmcli device wifi list
```

If the WiFi Access point that you want to connect is not initially listed try rescan to refresh the list. If after several tries the network still does not show, try rebooting by typing `sudo reboot`. After your machine has restarted:

```sh
sudo nmcli device wifi rescan
```

### Connecting to a WiFi network

We have bridged WiFi access points. Try to stay in the 5GHz network. The command to connect he JTN to an access point is

```sh
sudo nmcli device wifi connect <ssid_name> password <password>
```

Here are the WiFi access points we use:

```css
ssid="UCSDRoboCar5GHz"
password="UCSDrobocars2018"
ssid="UCSDRoboCar"
password="UCSDrobocars2018"
ssid="SD-DIYRoboCar5GHz" 
password="SDrobocars2017"
ssid="SD-DIYRoboCar"
password="SDrobocars2017"
```

So, to connect, an example command could look like:

```sh title="5GHz"
sudo nmcli device wifi connect UCSDRoboCar5GHz password UCSDrobocars2018
```

```sh title="2.4GHz"
sudo nmcli device wifi connect UCSDRoboCar password UCSDrobocars2018
```

### Get IP address of JTN

After connecting the JTN to a WiFi Access Point, let's find out the IP address the JTN is getting from the network so we can connect to it remotely. 

In a terminal:

```sh
ifconfig
```

You should see some output that contains the following line:

```sh
inet 192.168.222.167
```

!!! note

    Your IP address may be slightly different. It's important to use the corresponding `inet` IP address output onto your screen when configuring your JTN.

In the future, to connect to a new Access Point you may have to repeat these steps using a $\mu$USB cable connected between your PC and the JTN.

**Helpful Hint: You can add your phone as an Access Point so you always have a backup connection to your JTN. If you have your phone with you, you can connect to the JTN and then add another WiFi using SSH.**

### SSH communication (via WiFi)

Connect your PC to the JTN using WiFi (using example IP address from above) then enter the username and password.

```sh
ssh jetson@192.168.222.167
Username: jetson
Password: jetsonucsd
```

### Update WiFi power settings

Lets turn off the power saving for the WiFi device.


If your SSH connection is slow or lagging, make sure you have the power saving on the WiFi disabled.

```sh
sudo iw dev wlan0 set power_save off
```

Now let's install a small text editor called `nano`.

```
sudo apt-get update
sudo apt-get install nano
```
 
Lets make the change on the WiFi power saving settings persistent. We need to edit a file. 

```sh
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```

In the file, change

```
wifi.powersave = 3
```

to 

```
wifi.powersave = 2
```

## System Settings

Now lets change the name of the JTN (host name) and password of the JTN. We will configure the name of the JTN based on your Class `xxx` and Team `yy`. Hostname `ucsdrobocar-xxx-yy` where `xxx-yy` is your class and team number, ex: `148-05`, `190-01`, `190-TA1`, `190-TA2`
Example: `ucsdrobocar-148-05` for Team 5

To see your current computer information:

```sh
hostnamectl
```

*insert screenshot*

### Changing your hostname

Replace `xxx-yy` as shown above with your class name and team number.

```sh
sudo hostnamectl set-hostname ucsdrobocar-xxx-yy
```

Then there is one more place to change the hostname:

```sh
sudo nano /etc/hosts
```

*insert screenshot*

Change the name `jetson` here or any other name it may have for your JTN, for example

```sh hl_lines="2"
127.0.0.1   localhost
127.0.1.1   ucsdrobocar-xxx-yy
```

When making edits using `nano`, to save the file, first press ++ctrl+o++ to write out your changes. Press ++enter++ to save them under the same filename, and then press ++ctrl+x++ to exit.

