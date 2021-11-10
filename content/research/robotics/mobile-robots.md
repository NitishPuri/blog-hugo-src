---
title: Mobile Robots
mathjax: true
tags: [robotics]
categories: [notes]
lastmod: 2017-11-17
featuredImage: /images/papers/modularBots4.jpg
---

Very minimal notes on some papers or articles that I recently read. Mainly for logging purposes.

## Kinematics

### *tf*: The Transform Library {#tf}  
*Tully Foote*   
*Open Source Robotics Foundation*   

* The need for *tf* : to provide a standard way to keep track of coordinate frames and transform data within the entire system to a component user without requiring knowledge of all the coordinate frames.
* *Broadcaster* and *Listner* modules. 
* Closely related to *scene graphs*.   
![alt](/images/papers/tf1.jpg)   
* There are several different sources of information regarding various coordinate frames in a system, coming from sensors connected to hardware. This data can come at different frequencies.
* *tf* must accept asynchronous inputs and be robust to delayed or lost information.
* Must be robust to a distributed computing environment with unreliable networking and non negligible latency.
* Ability to dynamically change the relationship between frames to account for dynamic/varying structure.
* **Design**
* Transforms and frames are represented as a graph with transforms as edges and frames as nodes.
* The graphs can be disconnected, and must be directed ,acyclic and quickly searchable.
* Limiting the graphs to trees enables this.
* Difference from scene graphs: they are made to be iterated across periodically, while *tf* is designed to be queried for values asynchronously.
* History is also required.
* This data collectively is called a *Stamp*.
* *Broadcaster* broadcasts messages every time an update is heard about a specific transform.
* *Listner* collects the values and interpolates using SLERP, without assuming the presence of a future frame.
* The interpolation is a critical ability, as it allows the system to be asynchronous and robust to lost packets.
* *Transform Computation* using chaining. $T_c^a=T_a^b \times T_b^c$.
* **Strengths :** *Efficiency, Flexibility*
* **Extensions :**
* Support for velocity.
* Transforming data in time.

### Solving Kinematics Problems of a 6-DOF Robot Manipulator {#6dof}
*Computer Science Department, The University of Georgia : 2015*   
[*Source*](https://www.google.com/url?q=https%3A%2F%2Fwww.researchgate.net%2Ffile.PostFileLoader.html%3Fid%3D57cd4b20615e274c742de265%26assetKey%3DAS%253A402906919522304%25401473071903479&sa=D&sntz=1&usg=AFQjCNF8-U-44kQLISfar99yrIFTYElU9w)   


* An analytical approach for solving forward kinematics problem of a serial robot manipulator with six degrees of freedom and a specific combination of joints and links to formulate the position of gripper by a given set of joint angles.   
![alt](/images/papers/kuka_1.jpg)   
* The functional state of each joint related to its successive joint in the design of this robot is as follows:   
$$ R_1 \bot R_2 \parallel R_3 \bot R_4 \bot R_5 \bot R_6 $$   
in which $R$ indicates a revolute joint and the indices describe the position of the joint relative to the base of the robot.   
* Uses D-H parameter convention for assigning coordinate frames.
* D-H parameter analysis for Kuka KR60 can be found [here]({filename}kuka-kinematics.md)


## Robot Grasping

### Robotic Grasping and Contact: A Review {#grasping1}     
[*Source*](https://pdfs.semanticscholar.org/54f8/8557554d9a4e517f301cb170bd50cbe4cfc9.pdf)   

* Survey of work done in last two decades.
* Functions of Human hand 
    * Explore : *haptics*
    * Restrain : *fixturing*
    * Manipulation : *dexterous manipulation*
* Closure properties of grasps
    * Contact modelling the grap
* Force Analysis
* Contact model
    * Kinematics of contact
    * Contact compliance
* Measures of grasp performance.
* Grasping and the kinematics of the hand

### Universal robotic gripper based on the jamming of granular material {#grasping2}
*Eric Brown et. al*   
[*Source*](http://www.pnas.org/content/107/44/18809.full.pdf)   

* Multifindered hand in animals(or robots) require a central processor for multitude of decisions.
![alt](/images/papers/gripper_1.jpg)   
Jamming-based grippers for picking up a wide range of objects without the need for active feedback.   
(A) Attached to a fixed-base robot arm.   
(B) Picking up a shock absorber coil.    
(C) View from the underside.    
(D) Schematic of operation.    
(E) Holding force Fh for several three-dimensional-printed test shapes (the diameter of the sphere shown on the very left, 2R ¼ 25.4 mm, can be used for size comparison). The thin disk could not be picked up at all.   

### Dex-Net 2.0: Deep Learning to Plan Robust Grasps with Synthetic Point Clouds and Analytic Grasp Metrics {#grasping3}
*Jeffrey Mahler et. al*   
*Berkley*   
[*Source*](https://arxiv.org/abs/1703.09312)
[*Implementation and documentation*](http://berkeleyautomation.github.io/dex-net/)   

* Using physics based analyses to compute grasp configurations can be slow and error prone because of precision issues.
* Here we use a CNN on depth images and rendered point cloud data to estimate robust claw grasping configurations.
* Previous related work related to Grasp Planning is discussed.    
![alt](/images/papers/dexnet_1.jpg)   
*Dex-Net 2.0 Architecture. (Center) The Grasp Quality Convolutional Neural Network (GQ-CNN) is trained offline to predict the robustness candidate grasps from depth images using a dataset of 6.7 million synthetic point clouds, grasps, and associated robust grasp metrics computed with DexNet 1.0. (Left) When an object is presented to the robot, a depth camera returns a 3D point cloud, where pairs of antipodal points identify a set of several hundred grasp candidates. (Right) The GQ-CNN rapidly determines the most robust grasp candidate, which is executed with the ABB YuMi robot.*   


![alt](/images/papers/dexnet_2.jpg)   
*Graphical model for robust parallel-jaw grasping of objects on a table surface based on point clouds. Blue nodes are variables included in the state representation. Object shapes $\mathcal{O}$ are uniformly distributed over a discrete set of object models and object poses $\mathcal T_o$ are distributed over the object’s stable poses and a bounded region of a planar surface. Grasps $\mathbf{u} = (\mathbf{p}, \psi)$ are sampled uniformly from the object surface using antipodality constraints. Given the coefficient of friction $\gamma$ we evaluate an analytic success metric $S$ for a grasp on an object. A synthetic 2.5D point cloud $\mathbf y$ is generated from 3D meshes based on the camera pose $\mathcal T_c$, object shape, and pose and corrupted with multiplicative and Gaussian Process noise.*    

![alt](/images/papers/dexnet_3.jpg)   
*Dex-Net 2.0 pipeline for training dataset generation. (Left) The database contains 1,500 3D object mesh models. (Top) For each object, we sample hundreds of parallel-jaw grasps to cover the surface and evaluate robust analytic grasp metrics using sampling. For each stable pose of the object we associate a set of grasps that are perpendicular to the table and collision-free for a given gripper model. (Bottom) We also render point clouds of each object in each stable pose, with the planar object pose and camera pose sampled uniformly at random. Every grasp for a given stable pose is associated with a pixel location and orientation in the rendered image. (Right) Each image is rotated, translated, cropped, and scaled to align the grasp pixel location with the image center and the grasp axis with the middle row of the image, creating a 32 × 32 grasp image. The full dataset contains over 6.7 million grasp images*


* The performance of the network is compared with other machine learning models based on random forests and SVM along with previous DL based approaches.
* Some failure modes are identified as being unable to identify thin geometries and some problems with finding collision-free grasps in narrow parts of the object geometry, suggesting that performance can be improved with better depth sensing.

### Learning Hand-eye Coordination for Robotic Grasping with Deep Learning and Large-scale Data Collection {#grasping4}
[*Source*](https://people.eecs.berkeley.edu/~svlevine/papers/grasp_iser.pdf)   
*Sergey Levine, Peter Pastor, Alex Krizhevsky, and Deirdre Quillen*   
*Google*    

* Hand-eye coordination for robot grasping using monocular images.
* Consists of two components, first is a CNN to determine if a given motion would produce a successful grasp.   
![alt](/images/papers/grasp_1.jpg)   
*The architecture of our CNN grasp predictor. The input image $\mathbf{I}_t$, as well as the pregrasp image $\mathbf{I}_0$, are fed into a $6 \times 6$ convolution with stride 2, followed by $3 \times 3$ max-pooling and 6 $5 \times 5$ convolutions. This is followed by a $3 \times 3$ max-pooling layer. The motor command $v_t$ is processed by one fully connected layer, which is then pointwise added to each point in the response map of pool2 by tiling the output over the special dimensions. The result is then processed by 6 $3 \times 3$ convolutions, $2 \times 2$ max-pooling, 3 more $3 \times 3$ convolutions, and two fully connected layers with 64 units, after which the network outputs the probability of a successful grasp through a sigmoid. Each convolution is followed by batch normalization.*   

* Second is a continuous servoing mechanism that uses CNN to continuously update the robot's motor channels.

## Mobile Robots
### Modular and Reconfigurable Mobile Robots {#mobile1}   
[*Source*](http://128.173.188.245/publications/J13_Modular_Robots_JRAS.pdf)   

* Classification
    * ![alt](/images/papers/modularBots1.jpg)
* Modular robots with mobile configuration change(MCC)
    * S-bots
        * ![alt](/images/papers/modularBots2.jpg)
    * Uni-Rovers
        * ![alt](/images/papers/modularBots3.jpg)
    * JL-I and JL-II
        * ![alt](/images/papers/modularBots4.jpg)
    * Millibots
        * ![alt](/images/papers/modularBots5.jpg)
    * AMOEBA
        * ![alt](/images/papers/modularBots6.jpg)
* Modular robots with whole body locomotion(WBL)
    * Whole body locomotion in chain architecture
        * CONRO/PolyBot
            * ![alt](/images/papers/modularBots7.jpg)
        * GZ-I
        * CKBot
            * ![alt](/images/papers/modularBots8.jpg)
    * Whole body locomotion in a lattice architecture
        * Macro robots in a lattice architecture
            * *Crystalline*
                * ![alt](/images/papers/modularBots9.jpg)
            * *Odin*      
                * ![alt](/images/papers/modularBots10.jpg)
            * *I-Cubes*
                * ![alt](/images/papers/modularBots11.jpg)
            * *Catoms*
                * ![alt](/images/papers/modularBots12.jpg)
        * Mini robots in a lattice archtecture
            * ![alt](/images/papers/modularBots13.jpg)
        * Reconfigurable mechanisms in a lattice architecture
            * ![alt](/images/papers/modularBots14.jpg)
        * Whole body locomotion in a hybrid architecture
            * M-TRAN/iMobot
                * ![alt](/images/papers/modularBots15.jpg)
            * Molecubes
            * ATRON
                * ![alt](/images/papers/modularBots16.jpg)
            * YaMOR
            * SuperBot
                * ![alt](/images/papers/modularBots17.jpg)

## Swarm Intelligence

### Swarm Intelligence and Its Applications in Swarm Robotics {#swarm1}
*ALEKSANDAR JEVTIC, DIEGO ANDINA : Dec 2007*   
[*Source*](http://www.wseas.us/e-library/conferences/2007tenerife/papers/572-074.pdf)    

* An overview of swarm intelligence.   
![alt](/images/papers/swarm1.jpg)   
* Self-organizing mechanisms
    * Positive feedback
    * Negative feedback
    * Amplification of fluctuations
    * Multiple Interactions
* A *critical* number of individuals are required for *intelligence* to arise.
* **Particle Swarm Optimization**
    * Inspired by the flocking behavior of birds.
    * Composed of *simple* agents moving through multi-dimensional space, governed by a pull towards best known position to it and its neighbors.
* **Ant Colony Optimization**
    * Uses the *trail-laying-trail-following* behavior to communicate.
* Applications os SI,
    * Every problem, application, that in its base has some kind of optimization can be tackled 
    with SI techniques.
* **Swarm Robotics** : Inspired but not limited to SI.
* Criteria
    * *Autonomy* : autonomous units.
    * *Large number* : large homogeneous groups
    * *Limited capabilities* : incapable or inefficient on their own.
    * *Scalability and Robustness* : Loosing some units should not cause a failure.
    * *Distributed coordination* : Local and limited sensing and communication.
* Applications of SR
    * *Foraging* : includes collective exploration, path finding, efficient task allocation and collective transport. Examples, Search-and-rescue and terrain sample collection.
    * *Dangerous tasks* : Ex. mining.
    * *Exploration and mapping* : Collective exploration in space, extra-terrestrial planets and human veins and arteries. 

