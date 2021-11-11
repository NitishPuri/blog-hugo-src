---
title: Fundamentals of Computer Graphics, Peter Shirley, Part 6
use_math: true
tags: [graphics, notes, programming]
lastMod: 2017-08-17
---

## Chapter 12 : Data Structures for Graphics

### Triangle Meshes

Most real-world models are composed of complexes of triangles with shared vertices. These are usually known as *triangular meshes*, *triangle meshes*, or *triangular irregular networks (TINs)* and handling them efficiently is crucial to the performance of many graphics programs.   
The minimum information required for a triangle mesh is a set of triangles (triples of vertices) and the positions (in 3D space) of their vertices. But many, if not most, programs require the ability to store additional data at the vertices, edges, or faces to support texture mapping, shading, animation, and other operations. Vertex data is the most common: each vertex can have material parameters, texture coordinates, irradiances—any parameters whose values change across the surface. These parameters are then linearly interpolated across each triangle to define a continuous function over the whole surface of the mesh. However, it is also occasionally important to be able to store data per edge or per face.   

#### **Mesh Topology**

* Every edge is shared by exactly two triangles.
* Every vertex has a single, complete loop of triangles around it.

![alt](/images/fundcg/12_mesh1.png) 
![alt](/images/fundcg/12_mesh1_1.png)     
![alt](/images/fundcg/12_mesh2_1.png) 
![alt](/images/fundcg/12_mesh2.png)     
![alt](/images/fundcg/12_mesh3.png)     

**Indexed Mesh Storage**   
```Java
Triangle {
     vector3 vertexPosition[3]
}
```
![alt](/images/fundcg/12_mesh4.png)     

```Java
Triangle {
     Vertex v[3]
}
Vertex {
     vector3 position // or other vertex data
}
```

**Indexed triangle mesh**   
```Java
IndexedMesh {
     int tInd[nt][3]
     vector3 verts[nv]
}
```

#### **Triangle Strips and Fans**
![alt](/images/fundcg/12_mesh5.png)     
![alt](/images/fundcg/12_mesh6.png)     

#### **Data Structures for Mesh Connectivity**
Indexed meshes, strips, and fans are all good, compact representations for static meshes. However, they do not readily allow for meshes to be modified. In order to efficiently edit meshes, more complicated data structures are needed to efficiently answer queries such as:   
* Given a triangle, what are the three adjacent triangles?
* Given an edge, which two triangles share it?
* Given a vertex, which faces share it?
* Given a vertex, which edges share it?

The most straightforward, though bloated, implementation would be to have three types, `Vertex`, `Edge`, and `Triangle`, and to just store all the relationships directly:
```Java
Triangle {
     Vertex v[3]
     Edge e[3]
}
Edge {
     Vertex v[2]
     Triangle t[2]
}
Vertex {
     Triangle t[]
     Edge e[]
}
```

The fixed-size arrays in the Edge and Triangle classes suggest that it will be more efficient to store the connectivity information there. In fact, for polygon meshes, in which polygons have arbitrary numbers of edges and vertices, only edges have fixed-size connectivity information, which leads to many traditional mesh data structures being based on edges. But for triangle-only meshes, storing connectivity in the (less numerous) faces is appealing.   

#### **The Triangle-Neighbor Structure**

```Java
Triangle {
     Triangle nbr[3];
     Vertex v[3];
}
Vertex {
     // ... per-vertex data ...
     Triangle t; // any adjacent tri
}
```
![alt](/images/fundcg/12_mesh8.png)     
![alt](/images/fundcg/12_mesh9.png)     

```Java
Mesh {
     // ... per-vertex data ...
     int tInd[nt][3]; // vertex indices
     int tNbr[nt][3]; // indices of neighbor triangles
     int vTri[nv]; // index of any adjacent triangle
}
TrianglesOfVertex(v) {
     t = v.t
     do {
          find i such that (t.v[i] == v)
          t = t.nbr[i]
     } while (t != v.t)
}
```

#### **The Winged-Edge structure**

```Java
Edge {
     Edge lprev, lnext, rprev, rnext;
     Vertex head, tail;
     Face left, right;
}
Face {
     // ... per-face data ...
     Edge e; // any adjacent edge
}
Vertex {
     // ... per-vertex data ...
     Edge e; // any incident edge
}
```

**The Half-Edge Structure**   

```Java
HEdge {
     HEdge pair, next;
     Vertex v;
     Face f;
}
Face {
     // ... per-face data ...
     HEdge h; // any h-edge of this face
}
Vertex {
     // ... per-vertex data ...
     HEdge h; // any h-edge pointing toward this vertex
}
```

### Scene Graphs

![alt](/images/fundcg/12_scene1.png)     
As with the pendulum, each object should be transformed by the product of the matrices in the path from the root to the object:    

* **ferry** transform using $M_0$
* **car body** transform using $M_0M_1$
* **left wheel** transform using $M_0M_1M_2$
* **left wheel** transform using $M_0M_1M_3$

An efficient implementation can be achieved using a *matrix stack*, a data structure supported by many APIs. A matrix stack is manipulated using *push* and *pop* operations that add and delete matrices from the right-hand side of a matrix product.   
For example, calling:   
$$
push(\mathbf M_0)\\
push(\mathbf M_1)\\
push(\mathbf M_2)
$$   

creates the active matrix $\mathbf M = \mathbf M_0 \mathbf M_1 \mathbf M_2$. A subsequent call to *pop()* strips the last matrix added so that the active matrix becomes $\mathbf M = \mathbf M_0 \mathbf M_1$. Combining the matrix stack with a recursive traversal of a scene graph gives us:   

$$
\mathbf{function}\; traverse(node)\\
\quad push(\mathbf M_{local})\\
\quad \text{draw object using composite matrix from stack} \\
\quad traverse(left\;child)\\
\quad traverse(right\;child)\\
\quad pop()
$$

### Spatial Data Structures

![alt](/images/fundcg/12_spatial1.png)     

#### **Bounding Boxes**

#### **Hierarchical Bounding Boxes**
$$
\mathbf{if}\;\text{(ray hits root box)}\;\mathbf{then}\\
\quad \mathbf{if}\;\text{(ray hits left subtree box)}\; \mathbf{then}\\
\quad \quad \text{check three triangles for intersection} \\
\quad \mathbf{if}\; \text{(ray intersects right subtree box)}\; \mathbf{then}\\
\quad \quad \text{check other three triangles for intersection}\\
\quad \mathbf{if}\; \text{(an intersections returned from each subtree)} \mathbf{then} \\
\quad \quad \mathbf{return}\; \text{the closest of the two hits} \\
\quad \mathbf{else\,if}\;\text{(a intersection is returned from exactly one subtree)}\;\mathbf{then}\\
\quad \quad \mathbf{return}\; \text{that intersection} \\
\quad \mathbf{else}\\
\quad \quad \mathbf{return}\; false \\
\mathbf{else} \\
\quad \mathbf{return}\; false
$$

There are many ways to build a tree for a bounding volume hierarchy. It is convenient to make the tree binary, roughly balanced, and to have the boxes of sibling subtrees not overlap too much. A heuristic to accomplish this is to sort the surfaces along an axis before dividing them into two sublists.   

The quality of the tree can be improved by carefully choosing AXIS each time. One way to do this is to choose the axis such that the sum of the volumes of the bounding boxes of the two subtrees is minimized. This change compared to rotating through the axes will make little difference for scenes composed of isotopically distributed small objects, but it may help significantly in less well-behaved scenes. This code can also be made more efficient by doing just a partition rather than a full sort.   

#### **Uniform Spatial Subdivision**

* In hierarchical bounding volumes, each object belongs to one of two sibling nodes, whereas a point in space may be inside both sibling nodes.
* In spatial subdivision, each point in space belongs to exactly one node, whereas objects may belong to many nodes.

The grid itself should be a subclass of surface and should be implemented as a 3D array of pointers to surface. For empty cells these pointers are NULL. For cells with one object, the pointer points to that object. For cells with more than one object, the pointer can point to a list, another grid, or another data structure, such as a bounding volume hierarchy.   

![alt](/images/fundcg/12_spatial2.png)     

#### **Axis-Aligned Binary Space Partitioning**

A node in this structure contains a single cutting plane and a left and right subtree. Each subtree contains all the objects on one side of the cutting plane. Objects that pass through the plane are stored in in both subtrees. If we assume the cutting plane is parallel to the $yz$ plane at $x = D$, then the node class is:   

```
class bsp-node subclass of surface
     virtual bool hit(ray e + td, real t0, real t1, hit-record rec)
     virtual box bounding-box()
     surface-pointer left
     surface-pointer right
     real D
```
### BSP Trees for Visibility

If we are making many images of a fixed scene composed of planar polygons, from different viewpoints—as is often the case for applications such as games— we can use a binary space partitioning scheme closely related to the method for ray intersection discussed in the previous section. The difference is that for visibility sorting we use non–axis-aligned splitting planes, so that the planes can be made coincident with the polygons. This leads to an elegant algorithm known as the BSP tree algorithm to order the surfaces from front to back. The key aspect of the BSP tree is that it uses a preprocess to create a data structure that is useful for any viewpoint. So, as the viewpoint changes, the same data structure is used without change.

#### **Overview of BSP Tree Algorithm**
The BSP tree algorithm is an example of a **painter’s algorithm**. A painter’s algorithm draws every object from back-to-front, with each new polygon potentially overdrawing previous polygons.   

#### **Building the Tree**
$$
\mathbf{function} \; add ( \text{triangle}\;T )\\
\quad \mathbf{if}\,(f(\mathbf a) < 0\;and\;f(\mathbf b) < 0\;and\;f(\mathbf c) < 0)\;\mathbf{then} \\
\quad \quad \mathbf{if}\, (\text{negative subtree is empty})\;\mathbf{then}\\
\quad \quad \quad \text{negative-subtree} = node(T) \\
\quad \quad \mathbf{else} \\
\quad \quad \quad \text{negative-subtree}.add(T) \\
\quad \mathbf{else\,if}\, (f(\mathbf a) > 0 and f(\mathbf b) > 0 and f(\mathbf c) > 0)\; \mathbf{then}\\
\quad \quad \mathbf{if}\; \text{positive subtree is empty}\;\mathbf{then}\\
\quad \quad \quad \text{positive-subtree} = node(T) \\
\quad \quad \mathbf{else}\\
\quad \quad \quad \text{positive-subtree}.add (T) \\
\quad \mathbf{else} \\
\quad \quad \text{we have assumed this case is impossible}
$$   

![alt](/images/fundcg/12_bsp1.png) 
![alt](/images/fundcg/12_bsp2.png)     

A precision problem that will plague a naive implementation occurs when a vertex is very near the splitting plane. For example, if we have two vertices on one side of the splitting plane and the other vertex is only an extremely small distance on the other side, we will create a new triangle almost the same as the old one, a triangle that is a sliver, and a triangle of almost zero size. It would be better to detect this as a special case and not split into three new triangles. One might expect this case to be rare, but because many models have tessellated planes and triangles with  shared vertices, it occurs frequently, and thus must be handled carefully. Some simple manipulations that accomplish this are:

$$
\mathbf {function}\;add( \text{triangle}\; T ) \\
\quad fa = f(\mathbf a)\\
\quad fb = f(\mathbf b)\\
\quad fc = f(\mathbf c)\\
\quad \mathbf{if}\; (abs(fa) < \epsilon) \mathbf{then}\\
\quad \quad fa = 0 \\
\quad \mathbf{if}\; (abs(fb) < \epsilon) \mathbf{then}\\
\quad \quad fb = 0 \\
\quad \mathbf{if}\; (abs(fc) < \epsilon) \mathbf{then}\\
\quad \quad fc = 0 \\
\quad \mathbf{if}\; (fa \leq 0\;and\;fb \leq 0\;and\;fc \leq 0) \mathbf{then} \\
\quad \quad \mathbf{if}\; \text{(negative subtree is empty)}\;\mathbf{then} \\
\quad \quad \quad \text{negative-subtree} = node(T) \\
\quad \quad \mathbf{else} \\
\quad \quad \mathbf{negative-subtree}.add(T) \\
\quad \mathbf{else\,if}\;(fa \leq 0\;and\;fb \leq 0\;and\;fc \leq 0) \mathbf{then} \\
\quad \quad \mathbf{if}\; \text{(positive subtree is empty)}\;\mathbf{then}\\
\quad \quad \quad \text{positive-subtree} = node(T) \\
\quad \quad \mathbf{else} \\
\quad \quad \quad \text{positive-subtree}.add(T)\\
\quad \mathbf{else} \\
\quad \quad \text{cut triangle into three triangles and add to each side} 
$$

#### **Cutting Triangles**

$$
\mathbf{if}\; (fa * fc \geq 0)\;\mathbf{then}\\
\quad swap(fb, fc)\\
\quad swap(\mathbf b, \mathbf c) \\
\quad swap(fa, fb) \\
\quad swap(\mathbf a, \mathbf b) \\
\mathbf{else\,if}\; (fb ∗ fc \geq 0)\;\mathbf{then} \\
\quad swap(fa, fc) \\
\quad swap(\mathbf a, \mathbf c) \\
\quad swap(fa, fb) \\
\quad swap(\mathbf a, \mathbf b) \\
compute\;\mathbf A \\
compute\;\mathbf B \\
T_1 = (\mathbf a, \mathbf b, \mathbf A) \\
T_2 = (\mathbf b, \mathbf B, \mathbf A) \\
T_3 = (\mathbf A, \mathbf B, \mathbf c) \\
\mathbf{if}\; (fc \geq 0)\;\mathbf{then} \\
\quad \text{negative-subtree}.add(T1) \\
\quad \text{negative-subtree}.add(T2) \\
\quad \text{positive-subtree}.add(T3) \\
\mathbf {else} \\
\quad \text{positive-subtree}.add(T1) \\
\quad \text{positive-subtree}.add(T2) \\
\quad \text{negative-subtree}.add(T3) \\
$$   

#### **Optimizing the Tree**
Mostly depends upon the order or triangle being added.   

### Tiling Multidimensional Arrays

![alt](/images/fundcg/12_tile1.png) 
![alt](/images/fundcg/12_tile2.png)     

#### **One-Level Tiling for 2D Arrays**
![alt](/images/fundcg/12_tile3.png)     

$$
index = n^2(B_xb_y + b_x), \quad offset = y'n+x'.
$$   

Thus the full formula for finding the 1D  index element $(x, y)$ in an $N_x \times N_y$ array with $n \times n$
tiles is,   
$$\begin{align}
index & = n^2(B_xb_y + b_x) + y'n + x' \\
& = n^2((N_x /n)(y/n) + x/n) + (y\mod n)n + (x \mod n).
\end{align}$$   

This expression contains many integer multiplication, divide and modulus operations, which are costly on some processors. When $n$ is a power of two, these operations can be converted to *bitshifts* and *bitwise logical operations*. However, as noted above, the ideal size is not always a power of two.   

However, there is a simple solution; note that the index expression can be written as   
$$index  = F_x(x) + F_y(y),$$   
where,   
$$\begin{align}
F_x(x) & = n^2(x/n) + (x \mod n)\\
F_y(y) & = n^2(N_x/n)(y/n) + (y \mod n)n
\end{align}$$   

We tabulate $F_x$ and $F_y$, and use $x$ and $y$ to find the index into the data array. These tables will consist of $N_x$ and $N_y$ elements, respectively. The total size of the tables will fit in the primary data cache of the processor, even for very large data set sizes.   


## Chapter 13 : More Ray Tracing
*If you start with a bruteforce ray intersection loop, you’ll have ample time to implement an acceleration structure while you wait for images to render.*   

### Transparency and Refraction
*Snell's Law,*   

$$n\sin\theta = n_t\sin\phi$$   

![alt](/images/fundcg/13_ray1.png)     

$$\begin{align}\mathbf t = \sin\phi \mathbf b - \cos\phi \mathbf n\end{align}$$   

Since we can describe $\mathbf d$ in the same basis, and $\mathbf d$ is known, we can 
solve for $\mathbf b$:   
$$\begin{align}
\mathbf d &= \sin\theta \mathbf b - \cos\theta\mathbf n \\
\mathbf b &= \frac{\mathbf d + \mathbf n\cos\theta }{\sin\theta } \\
\end {align}$$   

This means we can solve for $\mathbf t$ with known variables,   
$$\begin{align}
\mathbf t &= \frac{n(\mathbf d + \mathbf n\cos\theta) }{n_t} - \mathbf n\cos\phi \\
&=\frac{n(\mathbf d - \mathbf n(\mathbf d \cdot \mathbf n)) }{n_t} - \mathbf n \sqrt{1 - \frac{n^2(1-(\mathbf d \cdot \mathbf n)^2)}{n_t^2}}
\end {align}$$   

![alt](/images/fundcg/13_ray2.png)     


**Shilck approximations for Fresnel equations,**   

$$\begin{align}
R(\theta) = R_0 + (1+R_0)(1-\cos\theta)^5,
\end {align}$$   

where $R_0$ is the reflactance at normal incidence:   
$$\begin{align}
R_0 = \left(\frac{n_t-1}{n_t+1}\right)^2
\end {align}$$   

![alt](/images/fundcg/13_ray3.jpg)     

$$
\mathbf{if}\; (\mathbf p is on a dielectric)\;\mathbf{then}\\
\quad \mathbf r = reflect(\mathbf d, \mathbf n ) \\
\quad \mathbf {if}\; (\mathbf d \cdot \mathbf n < 0)\; \mathbf {then} \\
\quad \quad refract(\mathbf d, \mathbf n, \mathcal n, \mathbf t) \\
\quad \quad c = −\mathbf d \cdot \mathbf n \\
\quad \quad k_r = k_g = k_b = 1 \\
\mathbf else \\
\quad k_r = exp(−a_rt) \\
\quad k_g = exp(−a_gt) \\
\quad k_b = exp(−a_bt) \\
\quad \mathbf {if}\; refract(\mathbf d, −\mathbf n, 1/\mathcal n, \mathbf t)\;\mathbf {then} \\
\quad \quad c = \mathbf t \cdot \mathbf n \\
\quad \mathbf {else} \\
\quad \quad \mathbf {return}\; k ∗ color(\mathbf p + t\mathbf r) \\
R_0 = (\mathcal n − 1)^2/(\mathcal n + 1)^2
R = R_0 + (1 − R_0)(1 − c)^5 \\
\mathbf {return}\; k(R\,color(\mathbf p + t\mathbf r) + (1 − R) color(\mathbf p + t\mathbf t))
$$

### Instancing

![alt](/images/fundcg/13_ray4.png)     
![alt](/images/fundcg/13_ray5.png)     

### Constructive Solid Geometry 

![alt](/images/fundcg/13_ray6.png) 
![alt](/images/fundcg/13_ray7.png)     

### Distribution Ray Tracing

#### **Antialiasing**

For a code hat samples $n \times n$ samples across each pixel:
$$
\mathbf{for\,each}\;\text{pixel}\;(i,j)\;\mathbf{do} \\
\quad c = 0 \\
\quad \mathbf{for}\; p = 0 \text{ to } n − 1 \mathbf{do} \\
\quad \quad \mathbf {for} q = 0 \text{ to } n − 1 \mathbf{do} \\
\quad \quad \quad c = c + \text{ray-color}(i + (p + 0.5)/n, j + (q + 0.5)/n)\\
c_{ij} = c/n^2
$$   
This is called *regular sampling*.   

One potential problem with taking samples in a regular pattern within a pixel is that regular artifacts such as moir´e patterns can arise.   

![alt](/images/fundcg/13_ray8.png)     

#### **Soft Shadows** 

![alt](/images/fundcg/13_ray10.png) 
![alt](/images/fundcg/13_ray9.png)     

$$
\mathbf{for\,each}\;  \text{pixel}(i, j)\;\mathbf{do} \\
\quad c = 0 \\
\quad \text{generate}\, N = n^2 \text{jittered 2D points and store in array} r[ ] \\
\quad \text{generate}\, N = n^2 \text{jittered 2D points and store in array} s[ ] \\
\quad \text{shuffle the points in array}\, s[ ] \\
\quad \mathbf{for} p = 0 \text{ to } N − 1\;\mathbf{do} \\
\quad \quad c = c + \text{ray-color}(i + r[p].x(), j + r[p].y(), s[p]) \\
\quad c_ij = c/N
$$

**A Shuffle routine,**   
$$
\mathbf {for}\; i = N − 1 \text{ downto } 1 \,\mathbf{do} \\
\quad \text{choose random integer $j$ between $0$ and $i$ inclusive} \\
\quad \text{swap array elements $i$ and $j$}
$$

#### **Depth of Field**

![alt](/images/fundcg/13_ray11.png)    
![alt](/images/fundcg/13_ray12.jpg)    
![alt](/images/fundcg/13_ray13.png)    

#### **Motion Blur** 

![alt](/images/fundcg/13_ray14.jpg)    





