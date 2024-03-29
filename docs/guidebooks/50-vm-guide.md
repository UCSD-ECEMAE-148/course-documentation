# Virtual Machine with DonkeyCar/DonkeySim AI Simulator

## Initial Setup
### Download and Install
Download [VMware Player](https://www.vmware.com/products/workstation-player.html) (Windows and Linux), Fusion Player (MacOS). *Not working for Mac M1 or M2 yet.*

A [VMware virtual machine image can be downloaded from here](https://drive.google.com/file/d/1aGVPzoEPYW0GxUnVGjzkiNsqqJFgZ7hb/view?usp=sharing). Download and unzip it **onto your host computer**.

Once unzipped the virtual machine image will take about 40 GB of disc space. If needed, clear some space from your computer, i.e. removing videos or other large media files. Alternatively, use a USB external drive if needed.

* If you unzip the image onto an external USB, you will have to ensure the USB is connected each time you run the virtual machine on VMware.  
* If VMware Player asks you "Did you copy or move these files?" select "I copied them."

You need a host machine with at least **8 GB RAM**. The virtual machine is configured by default to use 5 GB RAM. If your host machine has 16 GB or more, you will need to configure your settings in VMware to use 8 GB. You can change this setting in your virtual hardware configuration **while the virtual machine is not running**.

### Initial Boot of VM
Start the virtual machine using the VMware player (Windows, Linux) or Fusion Player (MacOS).
- If needed to boot, enable Virtualization on your BIOS/EFI
- On the VMware Player machine configuration, disable "Optimization for Virtualization."
- Example of settings for Fusion Player on Mac:

### Log into VMware virtual machine
Username: `ucsd` <br>
Password: `UcsdStudent`

### Copy/Paste Capabilitites
If cutting and pasting from your host computer to your virtual machine is not working, open a terminal in the Linux VM and run the following:

```sh
sudo apt-get autoremove open-vm-tools
sudo apt-get install open-vm-tools-desktop
sudo reboot
```

## Connecting Game Controller
Using a game controller will be the desirable way to control your DonkeyCar simulation.  
Examples of compatible game controllers include  
**PS3, PS4, Xbox, Logitech F710**.

The controller needs to be connected to the **host computer** using USB cable, Bluetooth or USB dongle.

Once the controller is connected to the host, the VMware Player should ask you where you want to connect the game controller ('Connect to the host' or 'Connect to a virtual machine'). **Select the UCSD-Ubuntu machine**.

The pop-up window should appear as one of the following:

*image here*

If you don't see this option, reconnect the controller.

To verify that the virtual machine can see the game controller, check the input for `js0`.

* Open a terminal in Linux machine and run the following command.

```sh
ls /dev/input
```

You should see:

```sh
by-id    event0  event2  event4  event6  js0   mouse0  mouse2
by-path  event1  event3  event5  event7  mice  mouse1  mouse3
```

Once you confirm that you can see `js0`, let's test the joystick controls.

* From a terminal:

```sh
sudo apt-get update
sudo apt-get install -y jstest-gtk
jstest /dev/input/js0
```

*image of joystick controls*

Move/press the game controller buttons to test which buttons trigger the corresponding input values, i.e. X, O, Triangle, Select/Share, Start/Options.

> Jack, what if my game controller works with the `jtest /dev/input/js0` but is not working with the DonkeyCar?

Not to worry, [there is a way to build a custom file](https://docs.donkeycar.com/parts/controllers/) for your game controller with the controls we use most.

### Custom controller (if needed/desired)
If you have a controller that is not listed above, or you are having trouble getting your controller to work, or you simply want to map your controller differently, see the [Custom Game Controller](https://docs.donkeycar.com/parts/controllers/#creating-a-new-or-custom-game-controller) documentation on DonkeyCar.

To discover or modify the button and axis mappings for your controller, you can use the [Joystick Wizard](https://docs.donkeycar.com/utility/donkey/#joystick-wizard) via the command line. 

The Joystick Wizard will write a custom controller named `my_joystick.py` to your `mycar` folder. To use the custom controller, set `CONTROLLER_TYPE="custom"` in your `myconfig.py`.

* From a terminal, navigate to your `mycar` directory.

```sh
cd ~/projects/d4_sim
```

This should be the directory with all of your Python configuration files, i.e. `manage.py`, `train.py`, `myconfig.py`.

* First, make sure the OS can access your device. The utility `jtest` can be usefull here. If needed:

```sh
sudo apt install joystick
```

You must pass this utility the path to your controller's device. Typically this will be `/dev/input/js0`. However, if it is not, you must find the correct device path and provide it to the utility. You will need this for the `createjs` command as well.

* From the current directory you have just navigated to, run the command

```sh
donkey createjs
```

This will create a file named `my_joystick.py` in your `/d4_sim` folder, next to your `manage.py`.

Modify `myconfig.py` to use your new controller.

```sh
atom myconfig.py
```

Find the line below and modify it as:

```python
CONTROLLER_TYPE="custom"
```

## DonkeyCar AI Framework

[DonkeyCar AI Framework Explained](https://ori.codes/artificial-intelligence/)

### How to launch the simulator

To start the DonkeySim:

* Use the File Explorer to navigate to `~/projects/DonkeySimLinux`.  
* Double click the file `donkey_sim.x86_64` to execute.  
* You should now see the DonkeySim ready for use.

Depending on the track we will be racing on, you need to train on the track that will be used during the race in order to successfully race against other people.

Some tracks you will see are `donkey-circuit-launch-track-v0`, `donkey-warren-track-v0`, `donkey-mountain-track-v0`.

*Specify track for current quarter*

### Customize your virtual robot racer

- From a terminal:

```sh
cd ~/projects/d4_sim
atom myconfig.py
```

This time you will pull up the `myconfig.py` file to investigate and edit more in-depth.

In your `myconfig.py` you may modify the following variables

```python
GYM_CONF["racer_name"] = "UCSD-148-YourName"
GYM_CONF["country"] = "USA"
GYM_CONF["bio"] = "Something_about_you, ex: Made in Brazil"
```

in addition to the `car_name` variable in the following line.

```py3
GYM_CONF = { "body_style" : "car01", "body_rgb" : (255, 205, 0), "car_name" : "UCSD-148-YourName", "font_size" : 30}
```

You can also change the color of the car to one of UCSD's colors, blue or gold. You can see how by examining the commented lines in the demo file below.

An example `myconfig.py` file:

``` py3 title="myconfig.py"
# 04Jan22
# UCSD mods to make easier for the UCSD students to use the Donkey-Sim
# the following uncommented lines where copied here from the body of myconfig.py below
DONKEY_GYM = True
# DONKEY_SIM_PATH = "remote"
DONKEY_SIM_PATH = "/home/ucsd/projects/DonkeySimLinux/donkey_sim.x86_64"
# DONKEY_GYM_ENV_NAME = "donkey-warren-track-v0"
DONKEY_GYM_ENV_NAME = “donkey-mountain-track-v0”
# UCSD yellow color in RGB = 255, 205, 0
# UCSD blue color in RGB = 0, 106, 150
GYM_CONF = { "body_style" : "car01", "body_rgb" : (255, 205, 0), "car_name" : "UCSD-148-YourName", "font_size" : 30} # body style(donkey|bare|car01) body rgb 0-255
GYM_CONF["racer_name"] = "UCSD-148-YourName"
GYM_CONF["country"] = "USA"
GYM_CONF["bio"] = "Something_about_you, ex: Made in Brazil"
#
# SIM_HOST = "donkey-sim.roboticist.dev"
SIM_ARTIFICIAL_LATENCY = 0
SIM_HOST = "127.0.0.1"  
# when racing on virtual-race-league use host "roboticist.dev"
# SIM_ARTIFICIAL_LATENCY = 30
# when training on the host machine, 
# set artificial latency to the value 
# when you ping roboticist.dev. When racing on 
# virtual-race league, use 0 (zero)
#
# When racing, to give the ai a boost, configure these values.
AI_LAUNCH_DURATION = 3     # the ai will output throttle for this many seconds
AI_LAUNCH_THROTTLE = 1     # the ai will output this throttle value
AI_LAUNCH_KEEP_ENABLED = True     # when False (default) you will need to hit the 
# AI_LAUNCH_ENABLE_BUTTON for each use. 
# This is safest. When this True, is active on each trip into "local" ai mode.
#
# JOYSTICK
# When using a joystick modify these by uncommenting USE_JOYSTICK_AS_DEFAULT = True
#
# USE_JOYSTICK_AS_DEFAULT = True  
# when starting the manage.py, when True, will not require a --js option to use the joystick
JOYSTICK_MAX_THROTTLE = 1.0      # this scalar is multiplied with the -1 to 1 throttle value to limit the maximum throttle. This can help if you drop the controller or just don't need the full speed available.
JOYSTICK_STEERING_SCALE = 0.8    # some people want a steering that is less sensitve. This scalar is multiplied with the steering -1 to 1. It can be negative to reverse dir.
AUTO_RECORD_ON_THROTTLE = True   # if true, we will record whenever throttle is not zero. if false, you must manually toggle recording with some other trigger. Usually circle button on joystick.
JOYSTICK_DEADZONE = 0.2      # when non zero, this is the smallest throttle before recording triggered.
# #Scale the output of the throttle of the ai pilot for all model types.
AI_THROTTLE_MULT = 1.0     # this multiplier will scale every throttle value for all output from NN models
#
```

### Get latency from remote server `roboticist.dev`

First, ping the remote server and take note of the average ping time, i.e. 30 ms.

``` sh
ping donkey-sim.roboticist.dev
```

*insert image of ping results*

In this example, since I am in the same network as `donkey-sim.roboticist.dev` my ping time is much smaller, around 0.5 ms. Assuming you are somewhere in the USA, you should get between 20-60 ms.

Write this value into your `myconfig.py` file as shown above.

``` py3
SIM_ARTIFICIAL_LATENCY = 30
```

**Remember**: This step is crucial to train your car for remote racing when you are not on the same network in the future. 

### Collecting data
For training your model, we will do a "behavioral cloning" AI.

Begin by driving the robot in the local simulator, aka the simulator on your virtual machine that is running on your host.

If you have not already, activate the donkey virtual environment.

```sh
conda activate donkey
```

If you have activated the venv correctly, your terminal line should now look something like this:

```
(donkey) ucsd@ucsd-virt-ub:~/$
```

Navigate to your DonkeyCar directory.

```sh
cd ~/projects/d4_sim
```

Now let's drive the robot to collect data automatically. In your terminal, run:

```sh
python manage.py drive
```

You should see the following output.

```sh
You can now go to http://localhost:8887 to drive your car.
Starting vehicle at 20 Hz
```

Open a web browser like Chrome (or Firefox on Linux).

```sh
http://localhost:8887
```

### Driving your robot

To control your car, you can either use the controller after configuring it [following the steps above](#connecting-game-controller), or you can drive with your keyboard and mouse from the web browser.

Some helpful settings for your joystick to improve controllability are outlined in the `# JOYSTICK` section of `myconfig.py`.

```python
# JOYSTICK
USE_JOYSTICK_AS_DEFAULT = True   # when starting the manage.py
JOYSTICK_MAX_THROTTLE = 1.0      # this scalar is multiplied with the -1 to 1
JOYSTICK_STEERING_SCALE = 0.7    # some people want a steering that is less sensitive
AUTO_RECORD_ON_THROTTLE = True   # if true, will record whenever throttle is not zero
CONTROLLER_TYPE='F710'           # (ps3|ps4|xbox|nimbus|wiiu|F710|rc3|MM1|custom)
JOYSTICK_DEADZONE = 0.0          # when non zero, this is the smallest throttle before recording triggered
```

> Hint: if your trained model is having trouble with accuracy when going around curves, try setting `AUTO_RECORD_ON_THROTTLE = False`
> and engage your throttle manually. This way there will not be any gaps in recording data.

Some advantages to using a game controller over your mouse/keyboard:

- Delete 100 last data points (at 20 Hz, this is equivalent to the last 5 seconds of driving data)
    - On PS3/PS4 this is usually "Triangle" button
    - On Logitech F710 this is usually "Y"
    - If you miss a turn or accidentally drive your vehicle into a wall, just remove that 5 seconds of recording and keep going
- Emergency Stop
- Easily switch operation modes from AI to manual 

Practice for a while and don't worry about not driving well initially. We'll show you how to delete all the data you've collected in one run so you can start with a clean set later.

We will be training the AI model in increments of approximately 20 laps, so count your laps as feasible.

To stop the DonkeyCar, use `Ctrl+C` like other Python code you've used so far.

### Deleting data

You can delete your entire data directory to start fresh if you'd like. This can be done with basic Linux commands you've recently learned.

Where is the data stored? Take a guess.

```sh
cd ~/projects/d4_sim
ls
```

Inside your directory you should see a folder called `/data`. Let's clean it up and start fresh.

```sh
rm -rf data
```

Then, create the directory again.

```sh
mkdir data
```

### Training and testing

OK, so you've recorded 20 laps of driving data. Now what?

It's time to train your model using all the data by giving it a name when running the `train.py` command. From your `/d4_sim` directory, run the command:

```sh
python train.py --model=models/14mar24_sim_160x120_20.h5 --type=linear --tub=./data
```

By executing this command with the `--model` option, you are telling the framework to train a new model called `14mar24_sim_160x120_20.h5` with the data you've collected, with the `20` tag at the end indicating how many laps you're using to train with.

Depending on your connection speed and the amount of laps you've recorded, this could take a while. Once your model has been trained, let's test it.

```sh
python manage.py drive --model=models/14mar24_sim_160x120_20.h5 --type=linear
```

This should open up the simulator as before. Press "Select" twice on your controller to start the AI model (or whichever button corresponds with your controller to activate AI drive).

### Does your model work?

A lot of things may have just happened.

- Does your car go berserk? Don't worry, there's a few things you can try.  
    - Add another 20 laps and train again
    - Go into your `myconfig.py` and modify the sections of the code that configure the driving behavior of your AI model.  
        - This could consist of increasing or decreasing the AI throttle boost, changing the max throttle, changing the throttle multiplier, etc.
- Does your car drive well but lose it around the corners?  
    - Check to make sure your `AUTO_RECORD_ON_THROTTLE` is set to `False`. This way you won't lose data when you release the gas around tight corners, and you can also manually choose when you want to record your driving and when to stop.

While driving your robot to collect data, it is a good idea to keep the terminal visible so you can see whether you are recording data. Sometimes you may accidentally turn recording off while you are driving and you may not know it.

As you continue adding data by driving more laps manually, you can rename the model as you train. For example, if you add 20 more laps and train again, you may choose to change the `20` tag to `40` on your next train.

```sh
python train.py --model=models/14mar24_sim_160x120_40.h5 --type=linear --tub=./data
```

> Remember to run this from your `/d4_sim` directory for it to work

Now let's test your model again with the new data.

```sh
python manage.py drive --model=models/14mar24_160x120_40.h5 --type=linear
```

### Training on a specific tub

You may also choose to drive your model and store data in a different tub each time you add new laps.

```sh
cd ~/projects/d4_sim
mkdir -p data/tub_2
python manage.py drive --model=models/14mar24_160x120_40.h5 --type=linear --tub=data/tub_2
```

This instance creates a new directory in the `/data` directory, then initiates your simulator and points to the new tub for your driving data to be recorded.

Add 20 more laps. Then, to train with your new data and transfer the training to your previous model, run the following command.

```sh
python train.py --tub=data/tub_1 --transfer=models/previous_model.h5 --model=models/new_model.h5
```

For this example, the command would be

```sh
python train.py --tub=data/tub_1 --transfer=models/14mar24_sim_160x120_40.h5 --model=models/14mar24_160x120_40.h5
```

## UCSD GPU Cluster

**Please do not use the GPU cluster until you demonstrate training on your local machine first. It is part of your deliverables for the class.**

