---
title: Notes On Opengl
tags: [graphics, opengl]
categories: [notes]
mathjax: true
lastMod: 2017-08-09
---

Starter notes on OpenGL/Transformations

## Transformations
*Translation* :  (x, y, z)   
$$
\begin{bmatrix}   
 1 & 0 & 0 & x \\\   
 0 & 1 & 0 & y \\\   
 0 & 0 & 1 & z \\\   
 0 & 0 & 0 & 1   
\end{bmatrix}
$$

*Scale* : (x, y, z)
$$
\begin{bmatrix}   
 x & 0 & 0 & 0 \\\   
 0 & y & 0 & 0 \\\   
 0 & 0 & z & 0 \\\   
 0 & 0 & 0 & 1   
\end{bmatrix}
$$

*Rotation x* : 
$$
\begin{bmatrix}   
 1 & 0 & 0 & 0 \\\   
 0 & cos(\theta) & -sin(\theta) & 0 \\\   
 0 & sin(theta) & cos(theta) & z \\\   
 0 & 0 & 0 & 1   
\end{bmatrix}
$$

*Rotation y* : 
$$
\begin{bmatrix}   
 cos(\theta) & 0 & sin(\theta) & 0 \\\   
 0 & 1 & 0 & 0 \\\   
 -sin(\theta) & 0 & cos(theta) & z \\\   
 0 & 0 & 0 & 1   
\end{bmatrix}
$$

*Rotation z* : 
$$
\begin{bmatrix}   
 cos(\theta) & -sin(\theta) & 0 & 0 \\\   
 sin(theta) & cos(theta) & 0 & 0 \\\   
 0 & 0 & 1 & z \\\   
 0 & 0 & 0 & 1   
\end{bmatrix}
$$

## Vertex Specifications
Specify vertex data.

- Create buffer Data: const float vertexPositions[] = {...}
- Initialize vertex Buffer:
    - `glGenBuffers(1, &posiionBufferObject)`
    - `glbindbuffer(GL_ARRAY_BUFFER, positionbufferObject)`
    - `glbufferData(GL_ARRAY_BUFFER, sizeof(vertexpositions), vertexpositions, BufferobjectUsageHint)`
        - BufferobjectUsagehint:
            - `GL__STATIC_DRAW`: Static data, only intend to set it once
            - `GL_STREAM_DRAW`: Dynamic data, intend to update it constantly, generally once per frame
    - `glbindbuffer(0)`
- Specify vertex data:
    - `glbindbuffer(GL_ARRAY_BUFFER, positionbufferobject)`
    - `glenablevertexattribarray(0)`
    - `glvertexattribarray(0, 4, GL_FLOAT, GL_FALSE, 0, 0)`
- Display:
    - `gldrawarrays(GL_TRIANGLES, 0, 3)`
- `glbufferSubData()`: Change buffer data value

Vertex Array Objects: OpenGL Objects that store all of the state needed to make one or more draw calls. This includes attribute array setup information from `glVertexAttribArray` , buffer objects used for attribute arrays, and `GL_ELEMENT_ARRAY_BUFFER` binding, which is a buffer object that stores the index arrays, if needed

## Pipeline
- Vertex Specification
- Vertex Proocessing and Vertex Shader
- Culling
- Rasterization
- Fragment Processing

