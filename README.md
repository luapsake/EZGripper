<img src="https://img.shields.io/badge/melodic-passing-green&style=plastic"> <img src="https://img.shields.io/badge/kinetic-passing-green&style=plastic"> <img src="https://img.shields.io/badge/indigo-passing-green&style=plastic">

# EZGripper

![ROS](https://img.shields.io/badge/-ROS-22314E?style=plastic&logo=ROS)

The `luapsake/EZGripper` is a development branch and not the official branch.  This branch has the following differences:
- All EZGripper meshes and URDF reflect the latest injection molded gripper (the primary benefit of this branch)
- It works through ROS Melodic.  It has issues on ROS Noetic.
- The directory structure has been updated to include a bit more hierarchy to support Gazebo
- `ezgripper_driver` should be ignored - do not use


These are the EZGripper ROS drivers.  If you are not using ROS, use https://github.com/SAKErobotics/SAKErobotics

## Install the EZGripper ROS Driver (Indigo, Kinetic or Melodic -- Not Noetic yet)

1) Install the python EZGripper [library](https://github.com/SAKErobotics/libezgripper).

2) Install dependencies:

	   $ sudo apt-get install ros-$ROS_DISTRO-joystick-drivers

3) Download code:

	   $ cd ~/catkin_ws/src
	   $ git clone https://github.com/SAKErobotics/EZGripper.git
	   $ git clone https://github.com/luapsake/EZGripper
	   $ cd ..
	   $ catkin_make

4) Setup parameters in `joy.launch` file
  - **`~port`** - serial device (like `/dev/ttyUSB0`) or tcp endpoint (like `192.168.0.200:5000`) to use.
  - **`~baud`** - baud rate of the serial device, not used for tcp.
  - **`grippers`** - definition of grippers on this serial bus:<br/>The gripper name to use for the action interface and the servo id of the gripper (several ids if several grippers are to be used as one group), for example,  `{left:[9], right:[10,11]}`.<br/>By default, SAKE Robotics delivers its grippers with address 1 for Duals and 1 and 2 for Quads and 57kbps.

5) Launch the node - example launch files to support various EZGripper configurations.  

	   $ roslaunch ezgripper_driver joy.launch
	  
	   # joy.launch is configured for a single servo gripper (dual) and the USB interface
	  
       $ roslaunch ezgripper_driver joy2.launch
 	   # joy2.launch is configured for two independent servos (quad independent) and the USB interface
	  
	   $ roslaunch ezgripper_driver joy2sync.launch
	   # joy2sync.launch controls two servos as if it were a single servo (quad dependent) and the USB interface
	  
       $ roslaunch ezgripper_driver joy_tcp.launch
       # joy_tcp.launch controls a single servo via TCP instead of USB
	
## Action API

The driver provides an implementation of the SimpleActionServer, that takes in [control_msgs/GripperCommand](http://docs.ros.org/indigo/api/control_msgs/html/action/GripperCommand.html) actions.
A sample client ([nodes/client.py](ezgripper_driver/nodes/client.py)) is included that provides joystick control using the action API.

## URDF Models

See README.md in the urdf directory.
https://github.com/SAKErobotics/EZGripper/tree/master/ezgripper_driver/urdf


## TroubleShooting

Serial connection issues:

	Error message: 'Serial' object has no attribute 'setParity'  --- this message indicates you have a new version of serial library that causes issues.  Do the following command to load an older pySerial library.
	$ sudo pip install "pySerial>=2.0,<=2.9999"
	
	Error message: permission denied (get accurate error message).  This indicates the user does not have privellages to use the /dev/ttyUSBx.  The solution is to add the <user> to the "dialout" group.  After executing the following command, reboot.
	$ sudo adduser <user> dialout
	reboot
	


	
	
	
