# Robotic arm - Pick & Place project [![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)

Using Gazebo and RViz to simulate robotic arm "kuka-KR210" with 6 degree of freedom, and perform forward and inverse kinematics to pick an object from any shelf and place it or drop it in another place.

Gazebo, a physics based 3D simulator extensively used in the robotics world.

RViz, a 3D visualizer for sensor data analysis, and robot state visualization.

## Contents :
1. Introduction
2. Installation
3. Forward Kinemtaics
4. Inverse Kinematics


## 1- Introduction :
The KUKA KR 210 industrial robot arm is a high payload solution for serious industrial applications. With a high payload of 210 kg and a massive reach of 2700 mm, the KR 210 KR C2 robot is ideal for a foundry setting. In fact, a foundry wrist  with IP 67 protection is available with the KR 210 KR C2 instead of the standard IP 65 wrist.

Make sure you are using robo-nd VM or have Ubuntu+ROS installed locally.
    
![Kuka-Kr210](http://www.xpert-meca.com/web/images/tfc1.jpg)      ![Kuka-Kr210](https://www.coriolis-composites.com/tl_files/_media/images/Products/Fiber%20Placement%20Robot/ABB%20Robot/ABB_Specs_Galery.jpg)


## 2- Installation :
### One time Gazebo setup step:
Check the version of gazebo installed on your system using a terminal:
```sh
$ gazebo --version
```
To run projects from this repository you need version 7.7.0+
If your gazebo version is not 7.7.0+, perform the update as follows:
```sh
$ sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
$ wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install gazebo7
```

Once again check if the correct version was installed:
```sh
$ gazebo --version
```
### For the rest of this setup, catkin_ws is the name of active ROS Workspace, if your workspace name is different, change the commands accordingly

If you do not have an active ROS workspace, you can create one by:
```sh
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_make
```

Now that you have a workspace, clone or download this repo into the **src** directory of your workspace:
```sh
$ cd ~/catkin_ws/src
$ git clone https://github.com/udacity/RoboND-Kinematics-Project.git
```

Now from a terminal window:

```sh
$ cd ~/catkin_ws
$ rosdep install --from-paths src --ignore-src --rosdistro=kinetic -y
$ cd ~/catkin_ws/src/RoboND-Kinematics-Project/kuka_arm/scripts
$ sudo chmod +x target_spawn.py
$ sudo chmod +x IK_server.py
$ sudo chmod +x safe_spawner.sh
```
Build the project:
```sh
$ cd ~/catkin_ws
$ catkin_make
```

Add following to your .bashrc file
```
export GAZEBO_MODEL_PATH=~/catkin_ws/src/RoboND-Kinematics-Project/kuka_arm/models

source ~/catkin_ws/devel/setup.bash
```

For demo mode make sure the **demo** flag is set to _"true"_ in `inverse_kinematics.launch` file under /RoboND-Kinematics-Project/kuka_arm/launch

In addition, you can also control the spawn location of the target object in the shelf. To do this, modify the **spawn_location** argument in `target_description.launch` file under /RoboND-Kinematics-Project/kuka_arm/launch. 0-9 are valid values for spawn_location with 0 being random mode.

You can launch the project by
```sh
$ cd ~/catkin_ws/src/RoboND-Kinematics-Project/kuka_arm/scripts
$ ./safe_spawner.sh
```

If you are running in demo mode, this is all you need. To run your own Inverse Kinematics code change the **demo** flag described above to _"false"_ and run your code (once the project has successfully loaded) by:
```sh
$ cd ~/catkin_ws/src/RoboND-Kinematics-Project/kuka_arm/scripts
$ rosrun kuka_arm IK_server.py
```
Once Gazebo and rviz are up and running, make sure you see following in the gazebo world:

	- Robot
	
	- Shelf
	
	- Blue cylindrical target in one of the shelves
	
	- Dropbox right next to the robot
	

If any of these items are missing, report as an issue.

Once all these items are confirmed, open rviz window, hit Next button.

To view the complete demo keep hitting Next after previous action is completed successfully. 

Since debugging is enabled, you should be able to see diagnostic output on various terminals that have popped up.

The demo ends when the robot arm reaches at the top of the drop location. 

There is no loopback implemented yet, so you need to close all the terminal windows in order to restart.

In case the demo fails, close all three terminal windows and rerun the script.


## 3- Forward Kinemtaics :

We use the forward kinematics to calculate the final coordinate position and rotation of end-effector

1. first we define our symbols 
![Symbols](https://github.com/mohamedsayedantar/Robotic-Arm-Pick-Place/blob/master/misc_images/symbols.png)

2. using the URDF file `kr210.urdf.xacro` we able to know each joint position and extract an image for the robot building the DH diagram.

![Robot](https://d17h27t6h515a5.cloudfront.net/topher/2017/July/5975d719_fk/fk.png)

3. then we can exctract the `Denavit - Hartenberg` `DH` parameters

i | alpha | a | d | theta
- | ----- | - | - | -----
1 |   0   | 0 | 0.75 | theta_1
2 |   -pi/2   | 0.35 | 0 | theta_2 - pi/2
3 |   0   | 1.25 | 0 | theta_3
4 |   -pi/2   | -0.054 | 1.5 | theta_4
5 |   pi/2   | 0 | 0 | theta_5
6 |   -pi/2   | 0 | 0 | theta_6
7 |   0   | 0 | 0.303 | theta_7

![DH-parameters](https://github.com/mohamedsayedantar/Robotic-Arm-Pick-Place/blob/master/misc_images/DH.png)









