# bwi

This repository contains top-level ROS packages for the Building Wide
Intelligence (BWI) project of the University of Texas at Austin
Computer Science Department.

## BWI Repository Hierarchy

Packages contained in various released BWI repostories may depend on
other packages at the same or lower levels.  Dependencies on packages
from higher-level repositories are not permitted.

From top to bottom, the released repositories are:

 * [bwi](http://wiki.ros.org/bwi)
 * [segbot](http://wiki.ros.org/segbot)
 * [bwi_common](http://wiki.ros.org/bwi_common)

## Installation

### From Source

You can install all the BWI components normally built from source on
either ROS Indigo or Kinetic.

First, install ROS
[Indigo](http://wiki.ros.org/indigo/Installation/Ubuntu), or
[Kinetic](http://wiki.ros.org/indigo/Installation/Ubuntu).

The Kinetic version is only supported on Ubuntu Xenial, and is
only partially functional.

Then, make sure the ROS_DISTRO environment variable is set correctly:

```
echo $ROS_DISTRO
```

It may already be.  If not, issue the appropriate one of these two
shell commands:

```
$ export ROS_DISTRO=indigo
```
or
```
$ export ROS_DISTRO=kinetic
```

Next, clone the source repositories:
```
$ source /opt/ros/$ROS_DISTRO/setup.bash
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws
$ wstool init src https://raw.githubusercontent.com/astrosaeed/bwi/temp/rosinstall/kinetic.rosinstall
```

Install all dependencies:
```
$ rosdep update
$ rosdep install --from-paths src --ignore-src --rosdistro $ROS_DISTRO -y
```

Then, build everything:
```
$ catkin build
$ source devel/setup.bash
```

Note that the **catkin build** command from the **python-catkin-tools**
package is *required* for building on ROS Kinetic.  On ROS Indigo, you
can still use **catkin_make** instead, although the newer build tool
is recommended.

### For Version 3 Robot

To use this code on the Version 3 Segway Robot, one must also define
some enviroment variables. 

```
echo "export SEGWAY_INTERFACE_ADDRESS=10.66.171.1" >> ~/.bashrc
echo "export SEGWAY_IP_ADDRESS=10.66.171.5" >> ~/.bashrc
echo "export SEGWAY_IP_PORT_NUM=8080" >> ~/.bashrc
echo "export SEGWAY_BASE_PLATFORM=RMP_110" >> ~/.bashrc
echo "export SEGWAY_PLATFORM_NAME=RMP_110" >> ~/.bashrc
```

##Usage

### Navigation

To launch bwi and all packages required for navigation
```
$ roslaunch bwi_launch segbot_bu.launch
```

If you need to drive the segbot with a controller use the bwi_joystick_teleop package. Double check that the joystick is in X mode and not D mode or it will not work as expected. You can check this by looking at the physical switch on the joystick. 
```
$ roslaunch bwi_joystick_teleop joystick_teleop.launch


### Making a map with the segbot

You'll need to turn on the base segway drivers followed by gmapping and rviz.
```
$ roslaunch segbot_navigation robot_with_gmapping_v3.launch
$ roslaunch bwi_joystick_teleop joystick_teleop.launch

```

You can see your map being created in real time by selecting the map topic in rviz. When you are done you can save your map with the map_server package.
```
$ rosrun map_server map_saver -f ~/<map_name>
```

That will save a map in your selected directory.


