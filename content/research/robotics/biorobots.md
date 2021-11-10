---
title: Biologically Inspired Robots
mathjax: true
tags: [robotics, biorobots]
categories: [notes]
lastmod: 2017-09-05
featuredImage: /images/papers/bioRobots2.jpg
---

## Bio Robots

### OpenRatSLAM : an open source brain-based SLAM system {#openratslam}
*Feb 2013*   
[*Source*](http://static.springer.com/sgw/documents/1388513/application/pdf/10-3.pdf)    

* *RatSLAM, OpenRatSLAM, SLAM, Navigation, Mapping, Brain-based, Appearance-based, ROS, Open-source, Hippocampus*
* RatSLAM is a navigation system based on the neural processes underlying navigation in the rodent brain, capable of operating with low resolution monocular image data. This paper describes OpenRatSLAM, an open source version of RatSLAM with bindings to ROS   
* SLAM(Simultaneous Localization and Mapping) , at the core based on SIFT or SURF features.
* This implementation is based on RatSLAM, leveraging tools like OpenCV and ROS. 
* Modular, detailed, integrated with ROS and *rviz*, works online and offline.
* RatSLAM
    * ![alt](/images/papers/ratSlam1.jpg)
    * *Pose Cells, Local View Cells and Experience Map*
* OpenRatSLAM, [code](https://code.google.com/archive/p/ratslam)
    * ![alt](/images/papers/ratSlam2.jpg)
    * Pose Cell Network : represents pose in response to odometric and local view connections. This also makes decisions about the experience map node and link creation.
    * Local View Cells : determines whether a scene is novel or familiar by image comparison techniques. Mostly based on template matching.
    * Experience map : manages graph building, graph relaxation and path planning.
    * Visual Odometry : For image only datasets, provides an odometric estimate based on changes in the visual scene. 
* OpenRatSLAM parameters and tuning
    * ![alt](/images/papers/ratSlam3.jpg) ![alt](/images/papers/ratSlam4.jpg)
    * Iterative tuning by minimizing loss.
* Using OpenRatSLAM
    * Examples of datasets this is used with, and some results.
* Future work
* [Watch In Action](https://www.youtube.com/watch?v=-0XSUi69Yvs)
* {{<youtube 0XSUi69Yvs>}}
    
### Biologically Inspired App*roaches* to Robotics {#roaches}
*March 1997*   
[*Source*](http://pdfs.semanticscholar.org/501b/9eb3c085a66abe4bdd56043fc802c21d0526.pdf)   

* Big gap between fantasy and reality in terms of Autonomous Robots.
* Inspirations from *insects* : agility, adaptability, simplicity
* Focuses on *walking* like an insect.
* From Biology to Robotics
    * Studies done at various levels of integration and inspiration.
* Distributed Gait Control
* A Distributed Neural Network Controller    
* A Stick Insect Controller
    * ![alt](/images/papers/bioRobots2.jpg)
* Evolved Locomotion Controllers
    * Use genetic algorithms to evolve the neural networks for controlling the locomotion.
* Rough Terrain Locomotion

### The First Takeoff of a Biologically Inspired At-Scale Robotic Insect {#insect}
*April 2008*   
[*Source*](http://www.micro.seas.harvard.edu/papers/TRO08_Wood.pdf)    

* *Actuators, aerial robotics, biologically inspired robotics, microrobotics*
* Goal is to create an insect-sized, *truly micro* air vehicle.
    * Harvard Microrobotic Fly
    * ![alt](/images/papers/insect1.jpg)
    * Fig. (a) Conceptual drawing highlighting the four primary mechanical and aero-mechanical components.   
    * Fig. (b) First insect-scale flying robot able to takeoff.
* INSECT-FLIGHT
    * Dipteran thoracic mechanics is discussed.
* Creation of a Robotic Insect
    * Actuation
        * Using peizoceramic materials.
    * Transmission
    * Airfoils 
* [Watch In Action](https://www.youtube.com/watch?v=olqaMw8bIb8)

### Towards Dynamic Trot Gait Locomotion—Design, Control, and Experiments with Cheetah-cub, a Compliant Quadruped Robot {#gait}
*Alexander Spröwitz , Alexandre Tuleu, Massimo Vespignani, Mostafa Ajallooeian, Emilie Badri, Auke Jan Ijspeert :July 2013*   
[*Source*](https://infoscience.epfl.ch/record/184991/files/Draft1sprowitz2013.pdf)    

* **Cheetah Cub :** novel compliant quadruped robot.
* [Watch In Action](https://www.youtube.com/watch?v=_luhn7TLfWU)   
* Fastest of its kind with speeds upto $1.42ms^{-1}$.   
![alt](/images/papers/cheetah1.jpg)   
* The implementation of *multi-segment*, *compliant legs* presents a major biological solution.   
![alt](/images/papers/cheetah2.jpg)   
![alt](/images/papers/cheetah3.jpg)   
* *Webots model description*   
![alt](/images/papers/cheetah4.jpg)   
* *Control* 
* Uses Central pattern generators(CPG)   
* *Results*
![alt](/images/papers/cheetah5.jpg)   
![alt](/images/papers/cheetah6.jpg)   

### Gait Pattern Generation and Stabilization for Humanoid Robot Based on Coupled Oscillators {#gait2}
*Inyong Ha, Yusuke Tamura, and Hajime Asama : Sep 2011*   
[*Source*](http://www.robot.t.u-tokyo.ac.jp/asamalab/publications/files/582.pdf)   


* Achieve balanced walking for a **DarwIn-OP** by gait pattern generation and stabilization using coupled oscillators.
![alt](/images/papers/darwin_1.jpg)
 


