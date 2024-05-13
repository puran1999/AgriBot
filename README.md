# AgriBot
A heavy duty industrial robot arm with limited accuracy for cost effective agricultural applications. 

# Mechanical System Overview
The current version of this robot arm weighs 26 Kg and has 3 Axes, which allows the end effector to move with 3 degrees of freedom in 3D space with a reach of around 1 metre. The end effector face has five M6 Allen bolts to mount any custom gripper and/or add more degrees of freedom. No position encoders are used in any axes, so there is no closed loop feedback. Instead, the end limit sensors are used to set home reference after the power up. Nema-24 stepper motors are used along with custom designed gearboxes to drive the robot joints.

![Unable to load image](/media/robot_arm_side_by_side.png)

# Eletronic System Overview

Stepper drive used, controls stepper motors

ESP32 used, controls drives with pulse and direction. Checks limit sensors and remembers current motor position.

A host PC or any other capable device connects to ESP32 via USB for sending commands and robot pose.

# Software System Overview

ESP32 is programmed with [grblHAL](https://github.com/grblHAL) firmware to optimally control motors using a [trapezoidal](https://in.mathworks.com/help/robotics/ug/design-a-trajectory-with-velocity-limits-using-a-trapezoidal-velocity-profile.html) motion profile.

The host computer sends commands and robot pose to ESP32 in [G-CODE](https://en.wikipedia.org/wiki/G-code) format. Many 3D printers and plotters use [common G-CODES](https://linuxcnc.org/docs/html/gcode/g-code.html), so there are many G-CODE sender softwares available for PC. Here, [LaserGRBL](https://lasergrbl.com/) is used which is free and easy to use.

The G-CODE files with ".nc" extention are used to write program for robot motion. The files can be created in any text editor. Here, [VS code](https://code.visualstudio.com/) is used along with a [G-code extention](https://github.com/scottmwyant/vscode-gcode) that makes editing easier with synax highlighting.

# Demo First Run

## Step 1: Setup Softwares
Install [LaserGRBL](https://lasergrbl.com/) and [VS code](https://code.visualstudio.com/) on a PC.

## Step 2: Setup Hardware
Power up the robot and make sure the surroundings have no objects nearby. Connect the USB cable to the PC or laptop, placed at a distance from robot for safety.

## Step 3: Connection
Open LaserGRBL on PC and select the correct COM port (tip: open device manager of windows to check the COM port number assigned to the USB serial connection). Set the baud rate to 1115200, and select the option "Grbl > connect" from the menu bar.

## Step 4: Homing
Press the home button (the button with a lens over a house icon). All the three axes of robot should start running to find the end limit sensor on one side.

## Step 5: Running
Select any G-CODE program, like this "demo.nc" from the menu bar option "File > Open File". Click the Run button (the small green triangle). The robot should now start running according to the program. Tip: increase the counter value near the run button to run the same program multiple times repeatedly, for example, during an exhibition display.

## Step 6: Editing (Optional)
To change the program or load any new program, open VS code and write the G-CODE program in a ".nc" extention file. Always save the file after making any changes, then select the same file again in LaserGRBL to run the program.

# Safety Tips

## RESET
## HOMING




