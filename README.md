Buzzmobile - The self driving parade float
================
[![PDF Status](https://www.sharelatex.com/github/repos/gtagency/buzzmobile/builds/latest/badge.svg)](https://www.sharelatex.com/github/repos/gtagency/buzzmobile/builds/latest/output.pdf)


##Subfolders
- sensors
  - Take in data from phyical things and spit out minimally processed data (generally)
  - could also be "lines" in that the sensor returns a set of lines
  - nodes
    - camera
      - courtesy of [usb_cam]
    - gps
      - courtesy of [NMEA_NavSat]
    - lidar
      - courtesy of [hokuyo_node]
    - encoder(s)
      - courtesy of RoboJackets
      - axle encoders provide PD values
      - steering encoders are just potentiometers
      - we may want to do additional processing, but that can come later
  - topics
    - ../image_raw
    - scan
- processors
  - take in sensor data, process it and provide information about the external world
  - these can essentially be used to build a world model
  - nodes
    - classifier
    - short range lane detector
    - image_projected
      - takes in image_raw data
      - provides processed data with magic geometry and color preprocessing
    - lane extractor
    - obstacle extractor
  - topics
    - road_class_train
    - road_class
    - ../image_projected
    - lanes
    - obstacles
    - blocked
- planners
  - take in world model information and emit real world data
  - for example "turn left 5 degrees"
  - nodes
    - stay right
    - driver
    - teleop_joy (human control)
  - topics
    - planned_path
    - planned_pose(s)
- actuator
  - takes in planner results ("go forward 10 feet", "turn left 5 degrees") and emit machine instructions for motor controllers, etc.
- test
  - Scripts and other test code, used for validating car subsystems.
- eleanor
  - Files used to run the existing power wheels car
  - contains:
    - buzzmobile_launch - Launch files for starting the car systems.
    - navigator - Navigator node used to navigate the car across a graph of waypoints.
    - steering/arduino - The arduino sketch for the steering system, built on an Arduino Uno and Arduino motor controller.
    - steering/ros - ROS nodes to control the steering arduino sketch using "joy"


##Topics
- ../image_raw
  - namespaced raw camera data
- road_class_train
  - the data used to train the road classifier
- road_class
  - ternary road classification output
- ../image_projected
  - namespaced projections of images
- scan
  - lidar laser scan depth information
- lanes
  - lanes, in terms of high level road lanes as detected
- obstacles
  - things that we consider unusual, obstacles
- planned_path
