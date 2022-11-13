# Simple arm movement
I wrote three nodes in this project :  
- The first node is called simple_mover. The simple_mover node does nothing more than publish joint angle commands to simple_arm.  
- The second node called arm_mover. The arm_mover node provides a service called safe_move, which allows the arm to be moved to any position within its workspace that has been deemed safe. The safe zone is bounded by minimum and maximum joint angles, and is configurable via the ROS parameter server.  
- The last node is the look_away node. This node subscribes to the arm joint positions and a topic where camera data is being published. When the camera detects an image with uniform color, meaning that it’s looking at the sky, and the arm is not moving, the node will call the safe_move service via a client to move the arm to a new position.
  
[![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)
## Uses RoboND-simple_arm
A mini-project to better explain pub-sub architecture in ROS

## How to Launch the simulation?
#### Clone the package in your repositry
```sh
$ cd /home/workspace/catkin_ws/src/
$ git clone https://github.com/udacity/RoboND-simple_arm.git simple_arm
```

#### Initiate a workspace
```sh
$ cd Simple-Arm-Movement/src
$ catkin_init_workspace
$ cd ..
```

#### Build the `simple_arm` package
```sh
$ cd /Simple-Arm-Movement/ 
$ catkin_make
```

#### After building the package, source your environment
```sh
$ cd /Simple-Arm-Movement/
$ source devel/setup.bash
```

#### Make sure to check and install any missing dependencies
```sh
$ rosdep install -i simple_arm
```

#### Once the `simple_arm` package has been built, you can launch the simulation environment using
```sh
$ roslaunch simple_arm robot_spawn.launch
```
## Testing the nodes
### Simple Mover: Build and Run
#### Building the Package
```sh
$ cd /Simple-Arm-Movement/ 
$ catkin_make
```
#### Running simple_mover
Assuming that your workspace has recently been built, you can launch simple_arm as follows:
```sh
$ cd /Simple-Arm-Movement/ 
$ source devel/setup.bash
$ roslaunch simple_arm robot_spawn.launch
```
Once the ROS Master, Gazebo, and all of our relevant nodes are up and running, we can finally launch simple_mover. To do so, open a new terminal and type the following commands:  
```sh
$ cd /Simple-Arm-Movement/ 
$ source devel/setup.bash
$ rosrun simple_arm simple_mover
```
### Arm Mover: Interact
#### Build and Launch
```sh
$ cd /Simple-Arm-Movement/ 
$ catkin_make
$ source devel/setup.bash
$ roslaunch simple_arm robot_spawn.launch
```
  
To view the camera image stream, you can use the command rqt_image_view (you can learn more about rqt and the associated tools on the RQT page of the ROS wiki):
```sh
$ rqt_image_view /rgb_camera/image_raw
```
#### Adjusting the view using arm mover
```sh
$ cd /Simple-Arm-Movement/ 
$ source devel/setup.bash
$ rosservice call /arm_mover/safe_move "joint_1: 1.3
joint_2: 1.3"
```
### Look Away: Interact
#### Build and Launch
```sh
$ cd /Simple-Arm-Movement/ 
$ catkin_make
$ source devel/setup.bash
$ roslaunch simple_arm robot_spawn.launch
```
After launching, the arm should move away from the grey sky and look towards the blocks. To view the camera image stream, you can use the same command as before:
```sh
$ rqt_image_view /rgb_camera/image_raw
```
To remake the action, open a new terminal and send a service call to point the arm directly up towards the sky (note that the line break in the message is necessary):
```sh
$ cd /Simple-Arm-Movement/ 
$ source devel/setup.bash
$ rosservice call /arm_mover/safe_move "joint_1: 0
joint_2: 0"
```
