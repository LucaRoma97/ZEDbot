# ZEDbot Simulation
This folder is relative to the simulation stage developed for my master thesis work. The project is called *Autonomous charging system for a three wheels robot*. The main goal of this project has been to find a way to drive the robot from a position to the station.
In this subsection, as first I will explain the algorithm to drive the robot from a defined position to the station, whenever it needs to be recharged. Then I'll explain how I have implemented the robot on Gazebo and I have translated the docking algorithm in C++ functions that communicate in ROS framework.


## Algorithm
After a reserch on the state-of-the-art of this technology with a comparison among the different sensor systems (e.g. infrared sensors, Lidar, GPS, RFID), I decided to take advantage of a **vision sistem** to detect an AR tag, placed on the station. 
For the development of the algorithm, I decided to divide the algorithm in 2 phases:

- **First Phase**: Approach to the perpendicular line to the tag.
- **Second Phase**: Straight approach to the tag in a safe and precise manner.

During the the docking operation, the robot should keep contact with the station, so the camera has to not lose the contact with the tag. One of the most important feature is the maximum orientation (Phi) the robot not lose the tag. In the first phase, the algorithm should make the robot rotate to reach the maximum orientation  (sigma -> Phi) in order to approach the perpendicular line as soon as possible and to complete the first phase. In this way, the robot can have enough space the complete the second phase. In the second phase the robot should reduce its speed until it stops in correspondence of the tag. 
First Phase             |  Second Phase
:-------------------------:|:-------------------------:
<img src="images/docking_algorithm.png" alt="alt text" width="350" height="350">  |  <img src="images/sign_second.png" alt="alt text" width="250" height="350">

In the first the algorithm checks:
- the **orientation sigma** to be as close as possible to the maximum orientation **Phi**
- the **angle** to understand from which part the the robot is approaching (from the left or from the right).

While in the second 

Moreover the *speed* varies with the distance. The higher the distance, the higher will be the speed, kind of an Hook's law. Thanks to this trick the robot can approach to the station in a safe manner. 

The flow chart of the algorithm:

<img src="images/flow_chart1.png" alt="alt text" width="550" height="650">


## Robot in Gazebo
The first step has been to implement the robot body with simple shapes (e.g. rectangulars, cylinders, spheres) in [mybot.xacro](https://github.com/LucaRoma97/mybot/blob/b5fe0e65ebe47e8366c96522a384b29c5b41f82d/src/my_bot_description/urdf/mybot.xacro). Then I added the camera functionality to sense the Gazebo environment and the differential drive one in order to separetely drive the two wheels. At the end, I built the environment recreating the Los Angeles apartment on Gazebo. 

<img src="images/simulation_initial.png" alt="alt text" width="550" height="350">


## Docking operation
The main functions developed for this algorithm are:
- **ar_track_alvar**: Detect the tag and publish the translation and rotation between camera and tag.
- **DistanceAngle**: Subscribe translation and rotation and provide distance, angle and orientation.
- **Docking**: Provide the left and right speeds to the wheels based on the distance, angle and orientation.

[*ar_track_alvar*](http://wiki.ros.org/ar_track_alvar) is the state-of-the-art in ROS for detecting this kind of tags. It publishes the rotation and translation information between camera and tag on the form of "[*tf2_msgs*](http://docs.ros.org/en/jade/api/tf2_msgs/html/msg/TFMessage.html)" message on the "*tf*" topic.

These vision data are subscribed and manipulated by the [**distanceangle**](https://github.com/LucaRoma97/distanceangle/blob/2b8b2acca71045aa31f86ee3143f056a31fe56de/src/distance_angle.cpp) node in order to extract the information about the *distance, angle* and *orientation* that are required for the algorithm. 
These information are published as a custom message [*DistanceAngle.msg*](https://github.com/LucaRoma97/distanceangle/tree/2b8b2acca71045aa31f86ee3143f056a31fe56de/msg). 

At the end, the [*DistanceAngle.msg*] message is subscribed by the [**dockingrobot**](https://github.com/LucaRoma97/dockingrobot/tree/a28d013236bdf047667e3db3c0b97a9e773126d9) function that, based on the data, sends the speed signals to the differential drive functionality of the robot and so to the Gazebo simulation. The function computes the maximum orientation based on these data 
This function is made of for loop and if statements. The if statements compare the robot orientation and the maximum orientation and check the sign of the angle.

Here I have a short video on my Youtube channel of the [Simulation](https://www.youtube.com/watch?v=KLvEFriNRXY).
