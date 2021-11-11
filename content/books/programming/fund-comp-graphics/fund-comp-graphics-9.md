---
title: Fundamentals of Computer Graphics, Peter Shirley, Part 9
use_math: true
tags: [graphics, notes, programming]
lastMod: 2017-08-17
---

## Chapter 23 : Tone Reproduction

- HDR : High Dynamic Range
- **Classification**
- **Dynamic Range**
- **Color**
- **Image Formation**
- **Frequency-Based Operators**
- **Gradient-Domain operators**
- **Spatial Operators**
- **Division**
- **Sigmoids**
- **Other Approaches**
    - Histogram Equalization
- **Night Tonemapping**
- **Discussion**
    - Global illumination generally produces HDR images,
    - Real-time rendering applications considerations: Sigmoid operators or Histogram equalization are good enough.

## Chapter 24 : Global Illumination

- **Particle Tracing for Lambertian Scenes**
    - Very randomized algorithm
    - For each of n particles, calculate power, compute a random point on source, compute  a random direction, while a random condition is satisfied, compute ray-intersection, add luminance to intersected surface, modify the power and ray, continue.
- **Path Tracing**
    - A ray is followed from through a pixel from the eye and scattered through the scene until it hits a luminaire.
- **Accurate Direct Lighting**
- Mathematical Framework
- Modeling a spherical Luminiare
- Non-diffuse luminiaries

## Chapter 25 : Reflection Models

- **Real-World Materials**
    - Smooth Dielectrics and Metals
    - Rough Surfaces
    - Diffuse Materials
    - Translucent Materials
    - Layered Materials
- **Implementing Reflection Models**
- **Specular Reflection Modes**
- **Smooth Layered Model**
- **Rough Layered Model**

## Chapter 26 : Computer Graphics in Games

- **Platforms**
    - Windows, consoles, browsers, handhelds , mobile
- **Limited Resources**
    - Processing Time
    - Storage
    - Development Resources
- **Optimization Techniques**
- **Game Types**
- **The Game Production Process**
- Asset Creation
    - Initial Modeling
    - Texturing
    - Shading
    - Lighting
    - Animation

## Chapter 27 : Visualization

- **Background**
- History
- Resource Limitations
    - Computational capacity
    - Human perceptual and cognitive capacity
    - Display Capacity
- **Data Types**
- Tables
    - *Items* and *attributes*
    - *Quantitative*, *Ordered* and *Categorical* attributes
- Graphs
- Dimension and Item count
- Data Transformation and Derived Dimensions
- **Human-Centered Design Process**
- Task Categorization - Iterative design process of requirement gathering.
- Abstraction
- Technique and Algorithm Design
- Validation
- **Visual Encoding Principles**
- Visual Channel Characteristics
- Color : Three different channels : Hue, Saturation and Lightness, Proper use of colormaps
- 2D vs 3D spatial layouts
    - Occlusion is a major problem in 3D
    - perspective distortion is another
- Text Labels
    - Text de-cluttering
- **Interaction Principals**
- Overview First, Zoom and Filter, Details on Demand
- Interactivity Costs : it has both power and cost.
- Animation
- **Composite and Adjacent views**
- Single Drawing
- Superimposing and Layering
- Glyphs
- Multiple Views
- **Data Reduction**
- Overviews and Aggregation
- Filtering and Navigation
- Focus + Context
- Dimensionality reduction
    - slicing, projection, dimensional filtering
    - Principal Component Analysis
- **Examples**
- Tables
    - General tables, table lens,
- Graphs
    - 2D, 3D, nodelink, adjacenecy matrix
- Trees
    - Treemaps, node-link
- Geographic
- Spatial Fields

## Chapter 28 : Spatial Field Visualization

- **2D Scalar Fields**
- Contours, height plot, color maps
- **3D Scalar Fields**
- Isosurfaces
    - Creating polygonal surfaces using marching cubes
    - Ray tracing
- Direct Volume rendering

