# Object Tracking and Detection using ROS
This repository contains a custom ROS package, `my_rosbot_2r_pkg`, designed to empower the Rosbot 2R with autonomous object detection and tracking capabilities. The package enables the robot to detect a specified object, move towards it, and maintain a fixed distance while keeping the object centered in its field of view. Through this functionality, the Rosbot can efficiently follow the object, ensuring consistent alignment and proximity, making it ideal for various robotics applications that require precise object interaction and tracking.

Using the `find_object_2d` package, the robot detects the object, processes its position relative to the robot through homography and perspective transformation, and adjusts the robot's movement accordingly. It also integrates OpenCV for visual feedback, drawing bounding boxes and centroids on the detected object. The robot’s distance to the object is monitored using either Lidar or TOF sensors, and the node continuously publishes velocity commands to keep the robot aligned and at the correct distance from the object.


## Gazebo World
The world contains different objects.

### Note 
1.  Details about cloning the repository are given at the end of this **readme file**

## Object detection using find_object_2d package
First, open a new terminal and install the find_object_2d package. Replace $ROS_DISTRO with the name of the ROS distribution installed on your system:
```bash
sudo apt-get install ros-$ROS_DISTRO-find-object-2d
```
Next, to store the data of objects for later recognition by the Rosbot, run the following launch file. Perform this action, after cloning the repository!
```bash
roslaunch object_teaching.launch teach:=true
```

A GUI will open with the Gazebo Simulation environment. Use the keyboard to control the robot (via the teleop twist keyboard node) and align it perfectly with the object you wish to detect.

To begin the teaching process, select Edit → Add object from scene... from the main toolbar. A new window will appear. Move the robot with the camera to capture as many features of the object as possible. Avoid capturing the surroundings. When ready, click Take picture.


In the next view, click Select region and choose only the part of the picture that covers the desired object, then click Next and then click End. Check the ID of the objects from the left windown.


Once you’ve added enough objects to the database, stop the camera by selecting Edit → Stop camera, then choose File → Save objects... from the main toolbar. Save the data inside the package in a separate folder.
!

## Object Tracking Logic
- The robot continuously rotates counterclockwise in the environment until it detects the desired object. Once found, it checks the distance to 
  the object and adjusts its position if the distance is outside the defined threshold.
- The robot strives to maintain focus on the center of the object. If the object's center is not perfectly aligned with the robot's center, the 
  robot rotates to correct its orientation.
- Robot uses both Lidar and RangeFinders to maintain a fixed distance from the object.  

## Run the Simulation
Run the object_following.launch file to initiate object tracking. In this simulation, the robot will rotate to detect and track a stop sign, maintaining a fixed distance from the object.
In order to launch the launch file (you should first navigate inside the launch folder of the package and then, use the following command): 
```bash
roslaunch object_following.launch
```
or if you want to use lidar for distance measurement:
```bash
roslaunch object_following.launch use_lidar:=true
```

## Create Ros Workspace
Open shell and execute the following commands:
```bash
mkdir object_tracking_ws
```
```bash
cd object_tracking_ws
```
# Clone the repository
```bash
sudo apt-get update
```
```bash
sudo apt-get install git
```
```bash
git clone https://github.com/AsadUllahYounus/Object-Tracking-ROS.git
```
```bash
cd rosbot_object_tracking_ws/src
```
Before running the simulation, please clone the rosbot repository inside the src folder of the ws.
```bash
git clone https://github.com/husarion/rosbot_ros.git -b noetic
```
```bash
cd ..
```
Run the below commands in root folder of the workspace
```bash
catkin_make 
```
```bash
source devel/setup.bash 
```

Press Enter and run the following command
```bash
roslaunch my_rosbot_2r_pkg object_following.launch use_lidar:=true
```
or
```bash
roslaunch my_rosbot_2r_pkg object_following.launch
```
