---
title: Fundamentals of Computer Graphics, Peter Shirley, Part 8
use_math: true
tags: [graphics, notes, programming]
lastMod: 2017-08-17
---

## Chapter 18 : Using Graphics Hardware

- What is Graphics Hardware,.?
- **Describing Geometry for Hardware.**
- Triangle Strips
- Indexed Drawing
- Indexed triangles, or some combination of vertex arrays and meshes
- *Display lists* and *Vertex Buffer Objects*
- **Processing Geometry into Pixels**
- Programming the pipeline
- Basic Execution Model
- Example of phong shading using a vertex shader and a then using a fragment shader
- General Purpose computing on GPU.

## Chapter 19 : Building Interactive Graphics Applications

- **The Ball Shooting Program**
- **Programming Models**
    - Control Driven Programming
    - Event Driven Programming
- GUI applications,
- The event driven ball shooting program
- The Modelview - Controller Architecture
- Examples:
    - Working with GUI APIs
    - Working with Graphics APIs
- Example implementations
    - OpenGL with FLTK
    - Direct3D with MFC
- Applying our results
    - Eg. Microsoft Powerpoint
    - Eg. Maya

## Chapter 20 : Light

- Radiometry
- Light, measured in joules
- Photons, *wavelength*, *frequency* and the *speed of light*.
    - Frequency is independent of the refractive index of the medium, wavelength and speed are not.
    - Energy = Plank's contant X frequency
- Spectral Energy = Energy for a spectral range = Energy/delta(wavelength)
- Power = (Spectral )energy per unit time
- Irradiance = How much light at this point = Power per unit area
- Radiance = How much light from this direction =  Irradianc/delta(solid angle)
- BRDF : Bidirectional Reflectance Distribution Function
- Lambertian surfaces
- Transport Equation.
- Photometry
- Luminance

## Chapter 21 : Color 

- **Colorimetry**
- Grassman's Law
- Cone responses
- Color matching experiments
- Standard Observers
    - Tristimulus functions(RGB)
- Chromaticity diagrams
- UV color values
- **Color Spaces**
- CIE X Y Z color space is the most popular and standard, but is not physically practical.
- Other color spaces are used and CIE XYZ is generally used for transforming in between different color spaces.
- Constructing a Matrix Transform
- **Chromatic Adaptation**
- **Color Appearance**

## Chapter 22 : Visual Perception

- Goal of a computer graphics system: to be *perceptually effective*.
- **Vision Science**
- **Visual Sensitivity**
- Human vision sees the changes in the light intensity or energy, not the absolute magnitude. yhis we are able to recognize spatial and temporal light patterns over a wide range of intensities.
- Fortunately, this is a good thing for computer graphics and display devices.
- Brightness and Contrast.
- Our currently available displays can not produce illumination differences as deep as our visual receptors.
- Color
    - HSV color space for human perception.
- Dynamic Range
    - *Light and dark adaptaion*
    - *Photopic*(bright light, only cones), *Scoptic*(dark light, only rods) and *mesopic*(overlap.) conditions
- Field-of-View and Acuity
- Motion
- **Spatial Vision**
- Frames of reference and Measurement Scales
- Accommodation and Convergence
- Binocular Density
- Motion Cues
- Pictorial Cues
- **Objects, Locations and Events**
- Object recognition
- Size and Distance
- Events
- **Picture Perception**

