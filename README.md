# ZEDbot Simulation
This folder is relative to the simulation stage developed for my master thesis work. The project is called *Autonomous charging system for a three wheels robot*. The main goal of this project has been to find a way to drive the robot from a position to the station.
In this subsection, as first I will explain the algorithm to drive the robot from a defined position to the station, whenever it needs to be recharged. Then I'll explain how I have implemented the robot on Gazebo and I have translated the docking algorithm in C++ functions that communicate in ROS framework.

## Algorithm
After a reserch on the state-of-the-art of this technology with a comparison among the different sensor systems (e.g. infrared sensors, Lidar, GPS, RFID), I decided to take advantage of a **vision sistem** to detect an AR tag, placed on the station. 
The vision data are manipulated in order to extract the information about the distance, angle and orientation between robot and station. To do so, I have subscribed the ar_track_alvar message "ar_pose_marker", which publishes the orientation and translation information in the form of "ARMarker - link!" mes.


## Robot in Gazebo
The first step has been to implement the robot body with simple shapes (e.g. rectangulars, cylinders, spheres). Then I added the camera functionality to represent the and the differential drive one in order to separetely drive the two wheels. 
![](images/ )

## Docking operation

ELENCHI PUNTATI, TABELLE
