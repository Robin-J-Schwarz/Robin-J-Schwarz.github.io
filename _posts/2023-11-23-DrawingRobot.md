---
layout: base
title: "Drawing an Image with a UR5 using ROS and OpenCV"
search: true
categories: 
  - Robotics
last_modified_at: 2023-11-23
---

During my exchange semester in Norway at the University of Agder (UiA), I took the Robotics course. The course focused on the concepts of industrial robot kinematics and path planning. For the final project, I and two fellow students programmed a UR5 robot to draw a picture with a marker. The project involved setting up the ROS environment on Ubuntu 20.04, the image processing pipeline using OpenCV and path planning from a random black and white image. With the path calculated, we then wrote the script to run the program first in a simulation and then on the physical UR5. 

### Results

The UR5 and the tooltip used:

![Robot](/assets/image/roboticsUIA/UR5andTooltip.png)

The reference image used for the project:

![Reference Image](/assets/image/roboticsUIA/ReferenceImage.png)

The Path planned by the algorithm:

![Path](/assets/image/roboticsUIA/Path.png)

The final result:

![Path](/assets/image/roboticsUIA/FinalResult.png)

**Read the full report [here](/assets/pdf/Report_Robotics_UiA.pdf)**
