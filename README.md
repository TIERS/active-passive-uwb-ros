# active-passive-uwb-ros
ROS Nodes for active (ToF) and passive (TDoA) localization using UWB modules

This repo utilizes the ToF and TDoA information of nodes with active and passive UWB tags, based on the firmware by 'https://github.com/TIERS/dynamic-uwb-firmware'. The code has been teste under Ubuntu 18.04/ROS Melodic only.

## Installation

This instructions are for Ubuntu 18.04 with ROS Melodic already installed.

Clone this repo into your catkin_ws (the code below creates a new catkin workspace named `uwb_ws` in your home folder):
```
mkdir -p  ~/uwb_ws/src && cd ~/uwb_ws/src
git clone https://github.com/TIERS/active-passive-uwb-ros.git
```

and install dependencies
```
sudo apt install python-dev python-pip
sudo -H python -m pip install --upgrade pip
```

Then build the workspace. We recommend using `catkin build`. Install it if needed with
```
sudo apt install python-catkin-tools
```

then run
```
cd ~/uwb_ws
catkin init
catkin build
```

## Get Started

Follow the steps in https://github.com/TIERS/dynamic-uwb-firmware to setup a UWB system with at least 6 active tag and 1 passive tag. 

## Scripts

This repository contains ROS nodes written in Python to interface with the two types of UWB Tags running custome firmware. 

- The node 'listener_interface.py' obtains the information of a passive (listener) tag and publishes both the node distances and difference of distance to it in different topic nodes.

- The node 'postioning.py' takes the distance between nodes and computes the position of all active nodes. Publishes positions o active nodes to `/uwb/anchor/NODE*/pose`

- The node 'tdoa_positioning.py' takes the difference of distance to a passive node and computes its possition. Publishes position of passive node to `/uwb/listener/NODE*/pose`

- The node 'node_select.py' takes the position of all nodes and computes which ones are active and passive. Publishes active node roles to `/uwb/anchor/nodes` and passive nodes to `/uwb/listener/nodes`

The passive tag publishes distances to `/uwb/tof/NODE*/NODE*/distance` and difference of distance from the listener to a pair of nodes to `/uwb/Listener_ID/tdoa/NODE*/NODE*/distance`. The __tag_name_ is given as a parameter

## Run

Then simply run for a passive tag (after checking which port it is connected to, usually starting from `/dev/ttyACM0`):

```
source ~/uwb_ws/devel/setup.bash
roslaunch listener/interface.launch
roslaunch positioning.launch
roslaunch node_select.launch
roslaunch tdoa_positioining.launch
``` 


