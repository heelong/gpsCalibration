# iMorpheus.ai - high availability sub-meter precise GPS
Through algorithm fusion of multiple data sources from different sensors such as lidar, radar, camera, gps, imu and point cloud, iMorpheus.ai brings about an high availability precision GPS measurement to outdoor robotics developer. iMorpheus integrate a number of advanced algorithm such as slam, kalman filter, ICP, feature selection and Gaussian Process. We believe precise GPS signal should obtained by computing from cloud rather than measure the satellite. 

## gpsCalibration copyrights
![License](https://img.shields.io/badge/License-Apache2.0-blue.svg)

## Current Version - alpha
The alpha version is a software package operate under off-line mode and using point clould and GPS to give you accurate GPS. Each GPS signal produced also comes with confidence level. 

## Installation Environment

### 1. Operating System
Prefer Ubuntu 14.04, Trusty

### 2. Robot Operating System - ROS
#### 2.1 About ROS
ROS provides libraries and tools to help software developers create robot applications. It provides hardware abstraction, device drivers, libraries, visualizers, message-passing, package management, and more. ROS is licensed under an open source, BSD license.
#### 2.2 Install ROS from
http://wiki.ros.org/cn/indigo/Installation/Ubuntu
#### 2.3 Learn more about ROS from
http://wiki.ros.org/ROS/Tutorials
#### 2.4 iMorpheus Perferred Version
ROS Indigo

### 3. Point Cloud Library - PCL
#### 3.1 About PCL
PCL is a large scale, open source project for 2D/3D image and point cloud processing.
#### 3.2 Install PCL from
http://pointclouds.org/downloads/
#### 3.3 Learn more about PCL from
http://pointclouds.org/documentation/
#### 3.4 iMorpheus Perferred Version
pcl-1.7.0

### 4.EIGEN
#### 4.1 About EIGEN
Eigen is a C++ template library for linear algebra: matrices, vectors, numerical solvers, and related algorithms.
#### 4.2 Install EIGEN from
http://eigen.tuxfamily.org/index.php?title=Main_Page
#### 4.3 Learn more about EIGEN from
http://eigen.tuxfamily.org/index.php?title=Main_Page
#### 4.4 iMorpheus Perferred Version
Eigen-3.3

## Module description
### 1. Modules
gpsCalibration has three modules include GPS, LOAM and Calibration.

#### 1.1 GPS module
This module processes GPS data and transforms them into local coordinate system.

#### 1.2 LOAM module
LOAM, Laser Odometry and Mapping, is a real-time method for state estimation and mapping using 3D lidar. The program contains two major threads running in parallel. 

The "odometry" thread computes motion of the lidar between two sweeps, at a higher frame rate. It also removes distortion in the point cloud caused by motion of the lidar. The "mapping" thread takes the undistorted point cloud and incrementally builds a map, while simultaneously computes pose of the lidar on the map at a lower frame rate. The lidar state estimation is combination of the outputs from the two threads.

To learn more about LOAM, please refer to [paper](http://www.frc.ri.cmu.edu/%7Ejizhang03/Publications/RSS_2014.pdf).

This package is a simple modified copy of loam_velodyne GitHub repository from laboshinl, which is again a modified copy of the original one release by Ji Zhang. His change on top of the original one is that he changed the scanRegistration.cpp to make it work with his dataset.

If you want to get laboshinl, pleaser go to:
https://github.com/laboshinl/loam_velodyne

We modified some codes in laserOdometry.cpp and scanRegistration.cpp to match the data generated by the lidar we use, and added some codes on transformMaintenance.cpp to get lidar's track with timestamp and map. Both are in txt format.

#### 1.3 Calibration module
This module calls GPS module and LOAM module and use their data as input.

We use timestamped point set registration to match GPS and LOAM tracks in two steps. First, we calculate long LOAM segments and find their least absolute deviations with GPS track by means of iteratively re-weighted least squares. Second, the weights from the first step are used to register overlapping short LOAM segments with GPS track by means of weighted least squares.

The final output is calibrated GPS track. We only work on 2D GPS tracks in this version. Altitudes are set to a constant for the purpose of demonstration in Google Earth.

#### 1.4 Flowchart
![image](https://github.com/iMorpheusAI/gpsCalibration/raw/master/demo/flowchart.jpg)
## How to compile and run the project
1.*Make sure that you have the message bag. it includes follow message types
  sensor_msgs/PointCloud2. and GPS coordinates matched with your run trail although 
  GPS is not accurate and not continuous in time.
  If you don’t have lidar or GPS data, don’t worry, we have some data in advance for you to have a try.*
* [point cloud data](http://www.imorpheus.ai/download/dataForDemo/bagDemo)
* [GPS data](http://www.imorpheus.ai/download/dataForDemo/original_gps_data)
  
2.*Open the globalConfig.py in directory "iMorpheusAI/"
  set needed file directory correctly.*

3.*In directory "iMorpheusAI", run command :*

```
$ catkin_make 
$ source setup.sh
$ mkdir data
$ cd data
$ touch bag_list.txt
  *add the point cloud data bag path*
```

4.*Okay, you can run ./run.py.*

5.*Finally, you get a global position system coordinates matched with your run trail. It is accurate and reliable!*

## About system input and output
### 1. Input
#### 1.1 .bag
A bag is a file format in ROS for storing ROS message data.
 
#### 1.2 GPS
The GPSRMC is protocol of GPSRMC's communication, and the format is:
$GPRMC,085223.136,A,3957.6286,N,11619.2078,E,0.06,36.81,180908,,,A\*57

### 2. Output
*The results of the program are stored in /data. We provide calibrated GPS in kml formats.
If you want to see GPS track in Google Earth, you need to use kml.
You can find Google Earth here: https://developers.google.com/kml/?hl=en-US.*

### 3. Example
#### 3.1 data download
* [point cloud data](http://www.imorpheus.ai/download/dataForDemo/bagDemo)
* [GPS data](http://www.imorpheus.ai/download/dataForDemo/original_gps_data)

#### 3.2 demo results
![image](https://github.com/iMorpheusAI/gpsCalibration/raw/master/demo/demo1.png)
![image](https://github.com/iMorpheusAI/gpsCalibration/raw/master/demo/demo2.png)
##### [See More](http://www.imorpheus.ai/demo/)

## Questions
  You can ask any question [here](https://github.com/iMorpheusAI/gpsCalibration/issues) or send us [emails](product@imorpheus.ai).
