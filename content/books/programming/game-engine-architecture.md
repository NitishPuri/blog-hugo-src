---
title: Game Engine Architecture
tags: [game-engine, graphics, architecture]
mathjax: true
lastMod: 2018-01-10
---

Based on : Game Engine Architecture by Jason Gregory

**All the technology that goes in making a AAA game!!!!**

## Foundations

### Introduction
* Industry overview
* Structure of a Typical Game Team 
    * Engineers
    * Artists
    * Game Designers
    * Producers
    * Other Staff 
    * Publishers and Studios
* What is a Game??
    * Video Games as Soft Real-Time Simulations
* What is a Game Engine??
    * Engine Differences across Genres
        * First-Person Shooter(Quake, Unreal Tournament, Half Life)   
        * Platformers and Other Third-Person Games(Gears of War, Dead Space, Red Dead Redemption, Uncharted)
        * Fighting Games(Soul Calliber, Tekken, Fight Night)
        * Racing Games(NFS, Gran Turismo, Mario kart)
        * Real Time Strategy(Warcraft, Starcraft, Command and Conquer, Age of Empires)
        * Massively Multiplayer Online Games(World of Warcraft, Destiny)
        * Player Authored Content(Mods)(Minecraft)
        * Other Games(sports, RPG, puzzle games, social sims, text based!!)
* Game Engine Survey 
    * The Quake Family of engines -- Quake, Sin, Medal of Honour (ID Software)
    * The Unreal Family of Engines -- Unreal Tournament, Gears of War (Epic Games)
    * The Half-Life Source Engine -- Half-Life, Team Fortress, Portal (Valve)
    * DICE's Frostbite -- Battelfield, Mass Effect, Need for Speed, Dragon Age (EA)
    * CryEngine -- Far Cry, Crysis, Warface (Crytek)
    * Son's PhyreEngine -- Demon's Souls, Dark Souls, Darksiders
    * Microsoft XNA Game Studio 
    * Unity 
    * 2D Engines, and others
* Open source and proprietary engines
* Runtime Engine Architecture
    * Target hardware, device drivers, operating system, 
    * middleware and third-party SDKs, 
        * DS and Algo, Graphics, Collision and Physics, Character Animation, Character Models
    * platform independence layer, core systems, resource manager, rendering engine, 
    * front end, profiling and debugging tools, human interface devices, audio, online multiplayer/networking
    * gameplay foundation systems, event subsystem, scripting system, AI foundations, game specific susbsystems
* Tools and asset pipeline
    * Digital content creation tools, Asset conditioning tools, World Editor, Resource database

### Tools of the Trade
* Version Control
* Visual Studio / IDE
* Profiling Tools
* Memory Leak and Corruption Detection

### Fundamentals of Software Engineering for Games
* C++ Review and Best Practices
* Data, Code and Memory
* Caching and Handling errors
* Pipelines, Caches and Optimization
    * Parallelism, memory caches, instruction pipelining and Superscalar CPUs, 

### 3D Math for Games
* Solving 3D Problems in 2D
* Points and Vectors
    * Coordinate spaces, vector operations
* Matrices
    * matrix operations, Homogeneous coordinates, transformations, matrices in memory, 
* Quaternions
    * quaternions operations, rotation, interpolation, 
* Comparison of Rotational Representations
    * Euler angles, matrices, axis + angle, quaternions,
* Other useful Mathematical Objects
    * Lines, Rays and Line Segments
    * Spheres
    * Planes
    * Axis-Aligned Bounding Box(AABB)
    * Frusta
* Hardware Accelerated SIMD Math
    * SSE Registors, intrinsics, vector-matrix multiplication
* Random Number Generation
    * Linear Congruential Generators
    * Mersenne Twister
    * Mother-of-all and XorShift

## Low Level Engine Systems

### Engine Support Systems
* Subsystem Start-Up and Shut-Down
    * Singleton Approaches
    * OGRE example
    * Naughty Dog's *Uncharted/The Last of Us* series.
* Memory Management
    * Issues:
        * Dynamic memory allocation
        * Memory access patterns
    * Custom Allocators
        * Stack based allocators
        * Pool allocators
* Containers
    * Container Operations, Iterators, Algorithmic Complexity
    * Building Custom Container Classes
* Strings
* Engine Configuration
    * Loading and Saving Options and Per-User options
        * Text files, XML, compressed binary, windows registry, command line options
        * environment variables, online user profiles
    * Examples
        * Quake's Cvars
        * OGRE : Windows INI
        * Naughty Dog : In-Game Menu Settings, Command Line Arguments, Scheme Data Definitions

### Resources and the File System
* File System
    * File names and Paths 
        * Across different operating systems, absolute and relative paths, search paths, path APIs
    * Basic File I/O
        * synchronous and asynchronous File I/O
* The Resource Manager
    * Offline Resource Management and the Tool Chain
        * Revision control of assets
        * The Resource database
            * Examples : UnrealEd, Naughty Dog's Builder, OGRE's Resource Management System
        * The Asset Conditioning Pipeline
    * Runtime Resource Management
        * Responsibilities :
            * Maintain a single copy in memory, Manage lifetime, load and unload, composite resources
            * Maintain referential integrity, manage memory usage, custom processing on load
            * Unified interface, Handles streaming
        * Resource File and Directory Organization
        * Composite Resources and Referential Integrity
        * Handling Cross-References between resources.
        * Post-Load Initialization

### The Game Loop and Real-Time Simulation
* The Rendering Loop
```C++
while(!quit) { 
    updateCamera(); 
    updateSceneElements();
    renderScene();
    swapBuffers();
}
```
* The Game Loop
    * Many interacting subsystems.
    * Each subsystem may work at a different refresh rate and synchronize with each other.
    * A Simple Example : *Pong*
```c
void main() // Pong 
{
    initGame();
    while(true) {   // game loop
        readHumanInterfaceDevices();
        if(quitButtonPressed) {
            break;  // break out of the game loop
        }
        movePaddles();
        moveBall();
        collideAndBounceBall();
        if(ballImpactSide(LEFT_PLAYER)) {
            incrementScore(RIGHT_PLAYER);
            resetBall();
        }
        else if(ballImpactSide(RIGHT_PLAYER)) {
            incrementScore(LEFT_PLAYER);
            resetBall();
        }

        renderPlayField();
    }
}
```
* Game Loop Architectural Styles
    * Windows Message Pumps
    * Event Based Updating
    * Callback-Driven Frameworks
```C++      
// The Framework!!
while(true) {
    foreach(frameListener) {
        frameListener.frameStarted();
    }
    renderCurrentScene();
    foreach(frameListener) {
        frameListener.frameEnded();
    }
    finalizeSceneAndSwapBuffers();
}
```
```C++
// A frame listener implementation
class GameFrameListener : public FrameListener 
{
public:
    virtual void frameStarted() {
        // Things that must happen before rendering
        pollInputDevices();
        updatePlayerControls();
        updateDynamics();
        resolveCollisions();
        updateCamera();
        // etc.
    }
    virtual void frameEnded() {
        // Things to do after scene rendering
        drawHud();
        // etc.
    }
}
```   

* Abstract Timelines
    * Real Time
    * Game Time
    * Local and Global Timelines
* Measuring and Dealing with Time
    * Frame Rate and Time Deltas
    * From Frame Rate to Speed
        * Old-School CPU Dependent Games
        * Updating Based on Elapsed Time
        * Using a Running Average
        * Governing the Frame Rate
        * Screen Tearing and V-Sync
    * Measuring Real Time with High-Resolution Timer
        * High Resolution Clock Drift : In multi-processor architecture
    * Time Units and Clock Variables
        * 64-Bit Integer Clocks
        * 32-Bit Integer Clocks
        * 32-Bit Floating-Point Clocks
        * Other Time Units
    * Dealing with Breakpoints
    * A Simple Clock Class
* Multiprocessor Game Loops
    * Multiprocessor Game Loop Architectures
        * Xbox 360, PlayStation 3, PlayStation 4, Xbox One
    * SIMD
    * Fork and Join
    * One Thread per Subsystem
    * Jobs
* Networked Multiplayer Game Loop
    * Client-Server 
    * Peer to Peer
    * Case Study : Quake II

### Human Interface Devices (HID)
* Types of Human Interface Devices
* Interfacing with HID
    * Polling
    * Interrupts
    * Wireless Devices
* Types of Inputs
    * Digital Buttons
    * Analog Axes and Buttons
    * Relative Axes
    * Accelerometers
    * 3D Orientation with the Wiimote or DialShock
    * Cameras
* Types of Outputs
    * Rumble
    * Force-Feedback
    * Audio
    * Other Inputs and Outputs
* Game Engine HID Systems
    * Typical Requirements
    * Dead Zone
    * Analog Singnal Filtering
    * Detecting Input Events
        * Button Up and Button Down
        * Chords
        * Sequences and Gesture Detection
    * Managing Multiple HIDs for Multiple Players
    * Cross-Platform HID Systems
    * Input Remapping
    * Context-Sensitive Controls
    * Disabling Inputs
* Human Interface Devices in Practice
    
### Tools for Debugging and Development
* Logging and Tracing
    * Formatted Output with `OutputDebugString()`
    * Verbosity
    * Channels
    * Mirroring Output to a File
    * Crash Reports
* Debug Drawing Facilities
    * Debug Drawing API
        * Should be simple, provide functionality to draw various primitives and control there properties.
        * Should allow primitives to be drawn from anywhere and in any coordinate space.
* In-Game Menus
* In-Game Console
* Debug Cameras and Pausing the Game
* Cheats
* Screenshots and Movie Capture
* In-Game Profiling 
    * Hierarchical Profiling
        * Measuring Execution times Hierarchically
    * Exporting to Excel
* In-Game Memory Stats and Leak Detection


## Graphics, Motion and Sound

### The Rendering Engine
* Foundations of Depth Buffered Triangle Rasterization
    * Describing a Scene : Using surfaces
        * Representations Used by High-End Rendering Packages
            * Parametric surface equations, patches
            * Triangle Meshes
                * Tesselation, LODs, dynamic tesselation, progressive meshes.
                * Triangle lists, index lists, strips and fans.
            * Model Space.
            * World space and Mesh instancing.
    * Describing the Visual Properties of a Surface. 
        * Introduction to Light and Color
        * Vertex Attributes
            * position, normal, tangent and bitangent, diffuse color, specular color, texture coordinates, skinning weights.
        * Vertex Formats
        * Attribute Interpolation
        * Textures
        * Materials
    * Lighting Basics
        * Local and Global illumination
        * The Phong Lighting Model
        * Modeling Light Sources
    * The Virtual Camera
        * View Space
        * Projections
        * The View Volume and the Frustum
        * Projection and Homogeneous Clip Space
        * Screen Space and Aspect Ratios
        * The Frame Buffer
        * Triangle Rasterization and Fragments
        * Occlusion and the Depth Buffer
* The Rendering Pipeline
    * Overview of the Rendeing Pipeline
        * How the Rendering Pipeline Transforms Data
        * Implementation of the Pipeline
    * The Tools Stage(*offline*)
        * Geometry and Material creation tools
    * The Asset Conditioning Stage(*offline*)
        * Load the data generated through DCC applications into engine ready format.
        * Index the data into scene-graphs or BSP tree to make it ready for the application.
    * A Brief History og the GPU
        * Software rendering
        * Fixed-function pipeline
    * The GPU Pipeline(*gpu*)
        * **Vertex Shader** - Transform and light  individual vertices from modelspace to clip space. Now has access to texture data as well.
        * **Geometry Shader** - Optional stage that can add/remove/modify primitives.
        * **Stream Output** - Permits to stream data at this point back into the pipeline for effects like hair simulation.
        * **Clipping** - Fixed function stage that chops the primitives to the frustum. User defined clip planes can be added.
        * **Screen Mapping** - Fixed stage that shifts and scales the vertices fromhomogeneous clip space into screen space.
        * **Triangle Set-Up** - Fixed initialization stage for rasterization hardware.
        * **Triangle Traversal** - Rasterize triangles and interpolate vertex attributes to generate per-fragment attributes. Fixed stage.
        * **Early z-Test** - Early z-test to avoid potentially expensive shading operating for occluded pixels. 
        * **Pixel Shader** - Programmable fragment shader. Gets per-fragment attributes and can access texture maps. Outputs a single color.
        * **Merging / Raster Operations Stage** - Non-Programmable, highly configurable *blending stage*, also known as *raster operations*. Alpha test, stencil test, alpha blending for translucent objects, decals?.
    * Programmable Shaders
        * Accessing Memory
        * Introduction to High-Level Shader Language Syntax
        * Effects
    * Antialiasing
        * Full Screen Antialiasing(FSAA)
        * Multi Sample Antialiasing(MSAA)
        * Coverage Sample Antialiasing(CSAA)
        * Morphological Antialiasing(MLAA)
    * The Application Stage
        * Visibility Determination
            * Frustum Culling, Occlusion and Potentially Visible Sets
            * Portals and Occlusion Volume(Anti-Portals)
        * Primitive Submission
        * Geometry Sorting
        * Scene Graphs
            * Quadtrees, Octrees and BSP trees      
        * Choosing a Scene Graph
* Advanced Lighting and Global Illumination
    * Image-Based Lighting
        * Normal Mapping
        * Heightmaps: Bump, Parallax and Displacement Mapping
        *  Specular/Gloss Maps
        * Environment Mapping
        * Three-Dimensional Textures
    * High Dynamic Range Lighting
    * Global Illumination
        * Shadow Rendering
            * Shadow maps and Shadow Volumes
        * Ambient Occlusion -- Static for objects, precomputed and stored  in textures.
        * Reflections
        * Subsurface Scattering
        * Precomputed Radiance Transfer
    * Deferred Rendering
    * Physically Based Shading
* Visual Effects and Overlays
    * Particle Effects
    * Decals
    * Environmental Effects
        * Skies
        * Terrain
        * Water
        * Overlays
            * Normalized coordinates
            * Reltive screen coordinates
            * Text and Fonts
    * Gamma Correction
    * Full Screen Post Effects
        * Motion Blur
        * Depth of field blur
        * Vignette
        * Colorization
* Further Reading
    * Graphics Gems / GPU Gems

### Animation Systems
* Types of Character Animation
    * Cel Animation -- Animated Sprites
    * Rigid Heirarchical Animation -- Rigging??
    * Per-Vertex Animation and Morph Targets
    * Skinned Animation -- Rigging!!
    * Animation Models as Data Compression Techniques
* Skeletons
    * The Skeletal Hierarchy
    * Representing a skeleton in memory
* Poses -- Transformations!
    * Bind Pose -- The base.
    * Local Poses 
        * Joint Scale
        * Representing a per-joint pose in memory
        * The Joint pose as a change os Basis
    * Global poses
        * Representing a global pose in memory.
* Clips
    * The Local Timeline
        * Pose interpolation and Continous Time
        * Time Units
        * Frame versus sample
        * Frames, Samples and Looping Clips
        * Normalized Time
    * The Global Timeline
    * Comparison of Local and Global Clocks
    * A Simple Animation Data Format
    * Continous Channel Functions
    * Metachannels
* Skinning and Matrix Palette Generation
    * Per-Vertex Skinning Information
    * The Mathematics of Skinning
* Animation Blending
    * LERP Blending
    * Applications of LERP Blending
        * Temporal Interpolation
        * Motion Continuity : Cross-Fading
        * Directional Locomotion
    * Complex LERP Blends
    * Partial-Skeleton Blending
    * Additive Blending!!!
        * Mathematical formulation
        * Additive Blending versus Partial Blending
        * Limitations of Additive Blending
* Applications of Additive Blending
    * Stance Variation
    * Locomotion Noise - *oh! so random!!*
    * Aim and Look-At
    * Overloading the Time Axis
* Post-Processing
    * Procedural Animations
    * Inverse Kinematics
    * Rag Dolls
* Compression Techniques
    * Channel Omission -- remove redundant information.
    * Quantization -- Use low precision data types by encoding information.
    * Sampling Frequency and Key Omission
    * Curve Based Compression
* Animation System Architecture
* The Animation Pipeline
    * Clip decompression and pose extraction
    * Pose blending
    * Global pose generation
    * Post-processing
    * Recalculation of global poses
    * Matrix palette generation --> Input to the rendering engine.
    ![0_fib](/images/gamearch/anim-pipe1.png)
    * Data Structures 
        * Shared Resource Data
            * Skeleton, Skinned meshes and Animation clips
        * Per-Instance Data
    * The Flat Weighted Average Blend Representation
        * Example : OGRE
        * Example : Granny
    * Blend Trees
    * Cross-Fading Architectures
    * Animation Pipeline Optimization
        * Optimization on the PS3
        * Optimization on the XBox and PS4
* Action State Machines
    * Animation States
        * State and Blend Tree Specifications
        * Custom Blend Tree Node Types
    * Transitions
        * Kinds of Transitions
        * Transition Parameters
    * The Transition Matrix
        * Implementing a Transition Matrix
    * State Layers
    * Control Parameters
    * Constraints
        * Attachments
        * Interobject Registration
        * Grabbing and Hand IK
        * Motion Extraction and Foot IK
        * Other Kinds of Contraints
* Animation Controllers

### Collision and Rigid Body Dynamics
* Do you want Physics in your Game?
    * Things you can do with a Physics System
    * Is Physics Fun?
        * Simulations (Sims)
        * Physics Puzzle Games
        * Sandbox Games
        * Goal-Based and Story-Driven Games
    * Impact of Physics on a Game
        * Design Impacts
        * Engineering Impacts
        *         








        


