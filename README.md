# AgriBot

A heavy duty industrial robot arm with limited accuracy for cost effective agricultural applications.

Modern robot arms are rich in extra features, that sometimes may not be required in some special applications. The high accuracy gearboxes, force feedback sensors, sophisticated control and extra degrees of freedom can be removed to reduce the cost of a functional robot arm that can be used in agricultural applications like fruit harvesting and stem pruning.

# Mechanical System

The image below shows the actual robot developed and the proposed design built in Fusion360.
![Unable to load image](/media/robot_arm_side_by_side.png)

The current version of this robot arm weighs 26 Kg and has 3 Axes, which allows the end effector to move with 3 degrees of freedom in 3D space with a reach of around 1 metre. The end effector face has five M6 Allen bolts to mount any custom gripper and/or add more degrees of freedom. No position encoders are used in any axes, so there is no closed loop feedback. Instead, the end limit sensors are used to set home reference after the power up.

Nema-24 stepper motors are used to drive the robot joints with the help of custom designed gearboxes to reduce the minimum required space.
![Unable to load image](/media/internal_gears_design.png)

# Eletronic System

The image below shows the PCB and stepper drives mounted inside the enclosure box. The red button is the emergency stop button which the operator can press to immediately stop all the motion in case of any accident.

![Unable to load image](/media/circuit.jpg)

The enclosure box has a power port and a control port. Reccomended power supply is 24V with at least 3A maximum current. Maximum voltage can go upto 44V DC. The control port has a USB-B interface to connect to any host computer or any other compatible device that can send the position waypoints to the robot. The ESP32 MCU receives the commands and moves the stepper motors accordingly by sending pulse-direction signals to the stepper drives.

# Software System Overview

ESP32 is programmed with [grblHAL](https://github.com/grblHAL) firmware to optimally control motors using a [trapezoidal](https://in.mathworks.com/help/robotics/ug/design-a-trajectory-with-velocity-limits-using-a-trapezoidal-velocity-profile.html) motion profile.

The host computer sends commands and robot pose to ESP32 in [G-CODE](https://en.wikipedia.org/wiki/G-code) format. Many 3D printers and plotters use [common G-CODES](https://linuxcnc.org/docs/html/gcode/g-code.html), so there are many G-CODE sender softwares available for PC. Here are some recommended free softwares: [UGS](https://winder.github.io/ugs_website/), [LaserGRBL](https://lasergrbl.com/) and [ioSender](https://github.com/terjeio/ioSender).

The G-CODE files with ".nc" extention are used to write program for robot motion. The files can be created in any simple text editor like Windows Notepad. [VS code](https://code.visualstudio.com/) can also be used along with a [G-code extention](https://github.com/scottmwyant/vscode-gcode) that makes editing easier with synax highlighting.

# Instructions for first demo run

## Step 1: Setup Software
Install ioSender software on PC or a laptop from [here](https://github.com/terjeio/ioSender/releases) (find the correct asset in latest release) or simply click [here](https://github.com/terjeio/ioSender/releases/download/2.0.44/ioSender.2.0.44.zip) to download the ioSender 2.0.44 version.

## Step 2: Setup Hardware
Power up the robot and connect the USB cable to the computer. The computer should be at a distance from the robot for safety. Make sure the surroundings of the robot have no objects nearby.

## Step 3: Connection
Open ioSender on the computer. During the first run, ioSender asks for connection settings. Select the correct COM port and set the baud rate to 115200. Click the OK button and ioSender should launch.

## Step 4: Homing
The ioSender should be showing ALARM state. This is because the robot controller does not know the current position of the robot joints yet. Press the "Home" button to initiate the calibration. All the three axes of robot should start running to find the end limit sensor on one side. The home is complete if all three sensors are found and the state changes from ALARM to IDLE.

## Step 5: Running programs
From the menu bar option, select "File > Load" and select [demo.nc](demo_prog.nc) program (or use any other G-CODE program). Click the "Cycle Start" button to run the program and the robot should start moving accordingly.

# Doing more

## Jogging
Moving the robot joints in small steps for testing is called jogging. This is helpful in programming, where the operator needs to go to a location, and then note down the waypoint coordinates. TO jog, make sure the robot is in IDLE state. Click the "Jog" tab at the rght of ioSender window. Click any increment or decrement buttons of the three axes to move the robot in small steps. Adjust the "Feed rate" for the speed of motion and "Distance" for the stepping amount. Since the robot joints are revolute, the axes values X,Y and Z are not cartesian coordinates, but spherical coordinates where each axes represents the angle of the corresponding joints.

## Programming
To change the program or load any new program, use any simple text editor like Windows Notepad or VS code and write the G-CODE program and save it in as a ".nc" extention file. Always save the file after making any changes, then click the reload file icon in the top left corner of ioSender window.

# Points to remember

## Emergency Stop
Press emergency stop button immediately if any unintentional motion is observed to prevent accidents. The robot control is open loop, so if some motor steps are missed due to stall (might happen during overload weight payload or under voltage or less current), the controller would not know. In such situation, press emergency stop. It is always mandatory to do homing after the emergency stop.
## HOMING
If home fails, it is likely that any sensor is damaged, or any axis is already beyond the specified limit. In later case, just power off the robot, move all axes to any centre position by hand, and power it again.
## POWER OFF
The robot does not have the 'normally active' brakes in the joints. Do not turn off the power to robot when the end effector is high, it may lead to arm falling under it's own weight. Instead, move the arm end effector near to the ground and then release power, or simply hold the end effector anywhere and remove power, then slowly move it to any resting position.




