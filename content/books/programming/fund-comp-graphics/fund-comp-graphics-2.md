---
title: Fundamentals of Computer Graphics, Peter Shirley, Part 2
use_math: true
tags: [graphics, notes, programming]
lastMod: 2017-08-17
summary: "Linear Algebra, Transformation Matrices and View Transformations   "
---

## Chapter 5 : Linear Algebra

![alt](/images/fundcg/5_liner1.png)
![alt](/images/fundcg/5_liner2.png)    

### Determinants

Determinants as the area of a parallelogram and volume of a parallelepiped.

![alt](/images/fundcg/5_deter1.png)    

We can see from Figure 5.6 that $|(b_c\mathbf{b}\mathbf{a})| = |c\mathbf{a}|$, because these parallelograms are just sheared versions of each other.   
Solving for bc yields $b_c =|\mathbf{ca}|/|\mathbf{ba}|$.   
An analogous argument yields $a_c =|\mathbf{bc}|/|\mathbf{ba}|$.   

This is the two-dimensional version of *Cramer’s rule* which we will revisit soon.   

### Matrices

A matrix is an array of numeric elements that follow certain arithmetic rules.   

**Matrix Arithmetic**   

Product with a scalar.   
Matrix addition.   
Matrix multiplication. Not commutative. Associative. Distributive.   

**Operations on Matrices.**   

Identity matrix.   
Inverse matrix.   

$$ (\mathbf{AB})^{-1} = \mathbf{B}^{-1}\mathbf{A}^{-1}$$   

Transpose of matrix,   
$$ (\mathbf{AB})^{T} = \mathbf{B}^{T}\mathbf{A}^{T}$$   

Also,   
$$ |{\mathbf{AB}}| = |{\mathbf{A}}|\,|{\mathbf{B}}| $$     
$$ |{\mathbf{A}^{-1}}| = \frac{1}{|{\mathbf{A}}|} $$     
$$ |{\mathbf{A}^T}| = |{\mathbf{A}}| $$      

**Vector operations in Matrix form.**    

*Special types of Matrices*   

* *Diagonal Matrix -* All non-zero elements occur along the diagonal.
* *Symmetrical Matrix -* Same as its transpose.
* *Orthogonal Matrix -* All of its rows(and columns) are vectors of length 1 and are orthogonal to each other. Determinant os such a matrix is either +1 or -1.   
The idea of an orthogonal matrix corresponds to the idea of an *orthonormal* basis, not just a set of orthogonal vectors.

### Computing with Matrices and Determinants

Determinants as areas.   
Laplace's Expansion.   
Computing determinants by calculating cofactors.   

**Computing inverses**   

$$\begin{eqnarray} \mathbf{A}^{-1} = \frac{1}{|{\mathbf{A}}|}\begin{bmatrix}   
 a^c_{11} & a^c_{21} & a^c_{31} & a^c_{41} \\\   
 a^c_{12} & a^c_{22} & a^c_{32} & a^c_{42} \\\   
 a^c_{13} & a^c_{23} & a^c_{33} & a^c_{43} \\\   
 a^c_{14} & a^c_{24} & a^c_{34} & a^c_{44} \\\   
\end{bmatrix} \end{eqnarray}$$   

**Linear Systems**   

$$\mathbf{Ax}=\mathbf{b}$$   

*Cramer's rule,*   
The rule here is to take a ratio of determinants, where the denominator is $|{\mathbf{A}}|$ and the numerator is the determinant of a matrix created by replacing a column of $\mathbf{A}$ with the column vector $\mathbf{b}$. The column replaced corresponds to the position of the unknown in vector $\mathbf{x}$. For example, $y$ is the second unknown and the second column is replaced. Note that if $|{\mathbf{A}}| = 0$, the division is undefined and there is no solution. This is just another version of the rule that if $\mathbf{A}$ is singular (zero determinant) then there is no unique solution to the equations.   

### Eigenvalues and Matrix Diagonalization

Square matrices have *eigenvalues* and *eigenvectors* associated with them. The eigenvectors are those *non-zero* vectors whose directions do not change when multiplied by the matrix. For example, suppose for a matrix $\mathbf{A}$ and vector $\mathbf{a}$, we have   
$$ \mathbf{Aa} = \lambda\mathbf{a} $$
This means we have stretched or compressed $\mathbf{a}$, but its direction has not changed.   
The scale factor $\lambda$ is called the eigenvalue associated with eigenvector $\mathbf{a}$.   

If we assume a matrix has at least one eigenvector, then we can do a standard manipulation to find it. First, we write both sides as the product of a square matrix with the vector $\mathbf{a}$:   
$$ \mathbf{Aa} = \lambda\mathbf{Ia} $$   
where $\mathbf{I}$ is an identity matrix. This can be rewritten   
$$\mathbf{Aa} − \lambda\mathbf{Ia} = 0$$   
Because matrix multiplication is distributive, we can group the matrices:
$$(\mathbf{A} − \lambda\mathbf{I}) \mathbf{a} = 0$$
This equation can only be true if the matrix $(\mathbf{A} − \lambda\mathbf{I})$ is singular, and thus its determinant is zero. The elements in this matrix are the numbers in $\mathbf{A}$ except along the diagonal.
Solving for $\lambda$ requires solving an *n*th degree polynomial in $\lambda$ . So we can only compute eigenvalues for matrices upto the order of *4 X 4* by analytical methods. For higher order matrices we need to use numerical solutions.   

An important special case where eigenvalues and eigenvectors are particularly simple is symmetric matrices (where $\mathbf{A} = \mathbf{A}_T$). The eigenvalues of real symmetric matrices are always real numbers, and if they are also distinct, their eigenvectors are mutually orthogonal. Such matrices can be put into *diagonal form*:   
$$\mathbf{A} = \mathbf{QDQ}_T$$

where $\mathbf{Q}$ is an orthogonal matrix and $\mathbf{D}$ is a diagonal matrix. The columns of $\mathbf{Q}$ are the eigenvectors of $\mathbf{A}$ and the diagonal elements of $\mathbf{D}$ are the eigenvalues of $\mathbf{A}$. Putting $\mathbf{A}$ in this form is also called the *eigenvalue decomposition*, because it decomposes $\mathbf{A}$ into a product of simpler matrices that reveal its eigenvectors and eigenvalues.   

**Singular Value Decomposition**   

$$\mathbf{A} = \mathbf{USV}_T$$   

Here $\mathbf{U}$ and $\mathbf{V}$ are two, potentially different, orthogonal matrices, whose columns are known as the left and right *singular vectors* of $\mathbf{A}$, and $\mathbf{S}$ is a diagonal matrix whose entries are known as the *singular values* of $\mathbf{A}$. When $\mathbf{A}$ is symmetric and has all non-negative eigenvalues, the SVD and the eigenvalue decomposition are the same.   
Also,   
$$\mathbf{M} = \mathbf{AA}_T = (\mathbf{USV}_T)(\mathbf{USV}_T)_T = \mathbf{US}(\mathbf{V}_TV)\mathbf{SU}_T = \mathbf{US}^2\mathbf{U}_T$$   

## Chapter 6: Transformation Matrices

### 2D Linear Transformations

$$\begin{bmatrix}a_{11} & a_{12} \\\ a_{21} & a_{22}\end{bmatrix} \begin{bmatrix}x \\\ y\end{bmatrix}
\begin{bmatrix}a_{11}x + a_{12}y \\\ a_{21}x + a_{22}y\end{bmatrix}$$   

This kind of operation, which takes in a 2-vector and produces another 2-vector by a simple matrix multiplication, is a *linear transformation*.   

**Scaling**   

$$ \mbox{scale}(s_x, s_y) = \begin{bmatrix} s_x & 0 \\\ 0 & s_y \end{bmatrix}$$

![alt](/images/fundcg/6_scale.png)   

**Shearing**   
$$ \mbox{shear-x}(s) = \begin{bmatrix} 1 & s \\\ 0 & 1 \end{bmatrix}$$   
$$ \mbox{shear-y}(s) = \begin{bmatrix} 1 & 0 \\\ s & 1 \end{bmatrix}$$   

![alt](/images/fundcg/6_shear.png)   

**Rotation**   

$$ \mbox{rotate}(\phi) = \begin{bmatrix} cos(\phi) & -sin(\phi) \\\ sin(\phi) & cos(\phi) \end{bmatrix}$$   
![alt](/images/fundcg/6_rotate.png)   

**Reflection**   
$$ \mbox{reflect-y} = \begin{bmatrix} -1 & 0 \\\ 0 & 1 \end{bmatrix}$$   
$$ \mbox{reflect-x} = \begin{bmatrix} 1 & 0 \\\ 0 & -1 \end{bmatrix}$$   

![alt](/images/fundcg/6_reflect.png)   

Reflection is in fact just a rotation by $\pi$ radians.   

**Composition and Decomposition of transforms**   
![alt](/images/fundcg/6_comp.png)   

*Not commutative.*   
![alt](/images/fundcg/6_comp2.png)   

**Decomposition of transforms**   
![alt](/images/fundcg/6_decomp.png)   

**Symmetric Eigenvalue Decomposition**   

$$\mathbf{A} = \mathbf{RSR}^T$$   

where $\mathbf{R}$ is an orthogonal matrix and $\mathbf{S}$ is a diagonal matrix; we will call the columns of $\mathbf{R}$ (the eigenvectors) by the names $\mathbf{v}_1$ and $\mathbf{v}_2$, and we’ll call the diagonal entries of $\mathbf{S}$ (the eigenvalues) by the names $\lambda_1$ and $\lambda_2$.   

![alt](/images/fundcg/6_svd.png)   

This symmetric *2 X 2* matrix has 3 degrees of freedom. One rotation angle and two scale values.

**Singular value Decomposition**   

![alt](/images/fundcg/6_svd2.png)   

$$\mathbf{A} = \mathbf{USV}^T$$   

In summary, every matrix can be decomposed via SVD into a rotation times a scale times another rotation. Only symmetric matrices can be decomposed via eigenvalue diagonalization into a rotation times a scale times the inverse-rotation, and such matrices are a simple scale in an arbitrary direction. The SVD of a symmetric matrix will yield the same triple product as eigenvalue decomposition via a slightly more complex algebraic manipulation.

**Paeth Decomposition of Rotations**   

$$\begin{bmatrix} cos\phi & -sin\phi \\\ sin\phi & cos\phi\end{bmatrix}  = 
\begin{bmatrix}1 & \frac{cos{\phi}-1}{sin\phi} \\\ 0 & 1\end{bmatrix} 
\begin{bmatrix}1 & 0 \\\ sin\phi & 1\end{bmatrix}
\begin{bmatrix}1 & \frac{cos{\phi}-1}{sin\phi} \\\ 0 & 1\end{bmatrix}$$   

![alt](/images/fundcg/6_decomp2.png)   

### 3D Linear Transformations   

**Scaling**   

$$ \mbox{scale}(s_x, s_y, s_z) = \begin{bmatrix} s_x & 0 & 0\\\ 0 & s_y & 0\\\ 0 & 0 & s_z\end{bmatrix}$$   

**Rotation**   

$$ \begin{eqnarray} 
\mbox{rotate-z}(\phi) & = 
&\begin{bmatrix} cos\,\phi & -sin\,\phi & 0\\\ sin\,\phi & cos\,\phi & 0 \\\ 0 & 0 & 1\end{bmatrix} \\\\
\mbox{rotate-x}(\phi) & = & \begin{bmatrix} 1 & 0 & 0\\\ 0 & cos\,\phi & -sin\,\phi \\\ 0 & sin\,\phi & cos\,\phi\end{bmatrix} \\\\
\mbox{rotate-y}(\phi) & = & \begin{bmatrix} cos\,\phi & -sin\,\phi & 0\\\ sin\,\phi & cos\,\phi & 0 \\\ 0 & 0 & 1\end{bmatrix}\end{eqnarray}$$   

**Shear**   

$$ \begin{eqnarray} \mbox{shear-z}(d_x, d_z) & = &\begin{bmatrix} 1 & d_y & d_z\\\ 0 & 1 & 0 \\\ 0 & 0 & 1\end{bmatrix} \end{eqnarray}$$   

As with 2D transforms, any 3D transformation matrix can be decomposed using SVD into a rotation, scale, and another rotation. Any symmetric 3D matrix has an eigenvalue decomposition into rotation, scale, and inverse-rotation. Finally, a 3D rotation can be decomposed into a product of 3D shear matrices.

#### Arbitrary 3D Rotations

$$ \begin{eqnarray} \mathbf{R}_{uvw} & = &\begin{bmatrix} x_u & y_u & z_u\\\ x_v & y_v & z_v \\\ x_w & y_w & z_w\end{bmatrix} \end{eqnarray}$$    

$$\mathbf{u}\cdot\mathbf{u} =\mathbf{v}\cdot\mathbf{v} = \mathbf{w}\cdot\mathbf{w}  = 1 , \\ 
\mathbf{u}\cdot\mathbf{v} =\mathbf{v}\cdot\mathbf{w} = \mathbf{w}\cdot\mathbf{u}  = 0$$   
$$\mathbf{R}_{uvw}\mathbf{u} = \begin{bmatrix}\mathbf{u}\cdot\mathbf{u} \\\ \mathbf{v}\cdot\mathbf{u} \\\ \mathbf{w}\cdot\mathbf{u}\end{bmatrix} = \begin{bmatrix} 1 \\\ 0 \\\ 0\end{bmatrix} = x$$   

#### Transforming Normal Vectors

Surface normal vectors are perpendicular to the tangent plane of a surface. These normals do not transform the way we would like when the underlying surface is transformed.   

![alt](/images/fundcg/6_normal.png)   

$$ \mathbf{n}_N = (\mathbf{n}^T\mathbf{M}^{-1})^T = (\mathbf{M}^{-1})^T\mathbf{n}$$   


### Translation and Affine Transformations

$$ \begin{bmatrix}x'\\\\y'\\\1\end{bmatrix} = \begin{bmatrix}m_{11} & m_{12} & x_t \\\ m_{21} & m_{22} & y_t \\\ 0 & 0 & 1 \end{bmatrix}
\begin{bmatrix}x \\\ y\\\1\end{bmatrix} = \begin{bmatrix}m_{11}x + m_{12}y + x_t \\\ m_{21}x + m_{22}y + y_t \\\  1 \end{bmatrix}$$   


For vectors that represent directions,   

$$ \begin{bmatrix}1 & 0 & x_t \\\ 0 & 1 & y_t \\\ 0 & 0 & 1 \end{bmatrix}\begin{bmatrix}x \\\ y\\\0\end{bmatrix} = \begin{bmatrix}x \\\ y \\\  0 \end{bmatrix}$$   

It is interesting to note that if we multiply an arbitrary matrix composed of scales, shears, and rotations with a simple translation (translation comes second), we get   

$$ \begin{bmatrix}1 & 0 & 0 & x_t \\\ 0 & 1 & 0& y_t \\\ 0 & 0 & 1 & z_t \\\ 0 & 0 & 0 & 1\end{bmatrix}\begin{bmatrix}a_{11} & a_{12} & a_{13} & 0 \\\ a_{21} & a_{22} & a_{23} & 0\\\ a_{31} & a_{32} & a_{33} & 0 \\\ 0 & 0 & 0 & 1\end{bmatrix} = \begin{bmatrix}a_{11} & a_{12} & a_{13} & x_t \\\ a_{21} & a_{22} & a_{23} & y_t\\\ a_{31} & a_{32} & a_{33} & z_t \\\ 0 & 0 & 0 & 1 \end{bmatrix}$$   


Thus, we can look at any matrix and think of it as a scaling/rotation part and a translation part because the components are nicely separated from each other. An important class of transforms are *rigid-body transforms*. These are composed only of translations and rotations, so they have no stretching or shrinking of the objects. Such transforms will have a pure rotation for the $a_{ij}$ above.   

### Inverses of Transformation Matrices

$$ \begin{array}0 \mathbf{M} &= &\mathbf{R}_1\mbox{scale}(\sigma_1, \sigma_2, \sigma_3)\mathbf{R}_2,\\\\
\mathbf{M}^{-1} & = &\mathbf{R}_2^{T}\mbox{scale}(1/\sigma_1, 1/\sigma_2, 1/\sigma_3)\mathbf{R}_1^{T} \end{array}$$   

### Coordinate Transformations


All of the previous discussion has been in terms of using transformation matrices to move points around. We can also think of them as simply changing the coordinate system in which the point is represented. For example, in Figure 6.19, we see two ways to visualize a movement. In different contexts, either interpretation may be more suitable.   

For example, a driving game may have a model of a city and a model of a car. If the player is presented with a view out the windshield, objects inside the car are always drawn in the same place on the screen, while the streets and buildings appear to move backward as the player drives. On each frame, we apply a transformation to these objects that moves them farther back than on the previous frame. One way to think of this operation is simply that it moves the buildings backward; another way to think of it is that the buildings are staying put but the coordinate system in which we want to draw them—which is attached to the car—is moving. In the second interpretation, the transformation is changing  the coordinates of the city geometry, expressing them as coordinates in the car’s coordinate system. Both ways will lead to exactly the same matrix that is applied to the geometry outside the car.

![alt](/images/fundcg/6_coord1.png)    

*Coordinate frame*,   

$$ \mathbf{p } + u\mathbf{u} + v\mathbf{v} + w\mathbf{w}$$   

$$\mathbf{p}_{xy} = \begin{bmatrix}\mathbf{u} & \mathbf{v} & \mathbf{e} \\\\ 0 & 0 & 1\end{bmatrix} \mathbf{p}_{uv}$$   

## Chapter 7 : Viewing

![alt](/images/fundcg/7_view1.png)    

### Viewing Transformations

* A *camera transformation* or *eye transformation*, which is a rigid body transformation that places the camera at the origin in a convenient orientation. It depends only on the position and orientation, or pose, of the camera.
* A *projection transformation*, which projects points from camera space so that all visible points fall in the range −1 to 1 in x and y. It depends only on the type of projection desired.
* A *viewport transformation* or *windowing transformation*, which maps this unit image rectangle to the desired rectangle in pixel coordinates. It depends only on the size and position of the output image.

![alt](/images/fundcg/7_view2.png)    

#### The Viewport Transformation

$$ \mathbf{M}_{vp} = \begin{bmatrix}\frac{n_x}2 & 0 & 0 & \frac{n_x-1}2 \\\ 0 & \frac{n_y}2 & 0 & \frac{n_y-1}2 \\\ 0 & 0 & 1 
 & 0 \\\ 0 & 0 & 0 & 1\end{bmatrix}$$

#### The Orthographic Projection Transformation

![alt](/images/fundcg/7_ortho1.png)    

$$ \mathbf{M}_{ortho} = \begin{bmatrix} \frac2{r-l}  & 0 & 0 & -\frac{r+l}{r-l} \\\ 0 & \frac{2}{t-b} & 0 & -\frac{t+b}{t-b} \\\ 0 & 0 & \frac{2}{n-f} & -\frac{n+f}{n-f} \\\ 0 & 0 & 0 &1\end{bmatrix} $$



$$ \begin{bmatrix}  x_{pixel} \\\ y_{pixel} \\\ z_{canonical} \\\ 1\end{bmatrix}  = (\mathbf{M}_{vp}\mathbf{M}_{ortho}) \begin{bmatrix} x \\\ y \\\ z \\\ 1\end{bmatrix}$$


#### The Camera Transformation

![alt](/images/fundcg/7_camera.png)    

* the eye position $\mathbf{e}$,
* the gaze direction $\mathbf{g}$,
* the view-up vector $\mathbf{t}$.   

$$ \begin{array}1 \mathbf{w} &=& -\frac{\mathbf{g}}{||\mathbf{g}||} \\\\
\mathbf{u} & = & \frac{\mathbf{t} \times \mathbf{w}}{|| \mathbf{t} \times \mathbf{w}||} \\\ 
\mathbf{v} & = & \mathbf{w} \times \mathbf{u} \end{array}$$


$$ \begin{array}0 \mathbf{M}_{cam} &=& \begin{bmatrix} \mathbf{u} & \mathbf{v} & \mathbf{w} & \mathbf{e} \\\ 0 & 0 & 0 & 1\end{bmatrix}^{-1} \\\ &=& \begin{bmatrix} x_u & y_u & z_u & 0 \\\ x_v & v_v & z_V & 0 \\\ x_w & y_w & z_w & 0 \\\ 0 & 0 & 0 &1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 & -x_e \\\ 0 & 1 & 0 &-y_e \\\ 0 & 0 & 1 & -z_e \\\ 0 & 0 & 0 & 1\end{bmatrix}\end{array}$$   


*The algorithm:*   

$$ construct \;\mathbf{M}_{vp}\\\\
construct\;\mathbf{M}_{ortho}\\\
construct\;\mathbf{M}_{cam} \\\
\mathbf{M} = \mathbf{M}_{vp}\mathbf{M}_{ortho}\mathbf{M}_{cam}\\\
\mathbf{for}\; each \;line \;segment (a_i, b_i) \;\mathbf{do}:\\\
\quad     \mathbf{p} = \mathbf{Ma}_i \\\
\quad     \mathbf{q} = \mathbf{Mb}_i \\\
\quad     drawline(x_p, y_p, x_q, y_q) $$   

### Projective Transformations

$$ y_s = \frac{d}{z}y$$   

![alt](/images/fundcg/7_project1.png)    

$$ \begin{bmatrix} \bar x \\\ \bar y \\\ \bar z \\\ \bar w\end{bmatrix}  = \begin{bmatrix} a_1 & b_1 & c_1 & d_1 \\\ a_2 & b_2 & c_2 & d_2 \\\a_3 & b_3 & c_3 & d_3 \\\ e & f & g & h \\\ \end{bmatrix} \begin{bmatrix} z \\\ y \\\ z \\\ 1\end{bmatrix}$$   

$$ (x', y', z') = (\bar x/\bar w, \bar y/\bar w, \bar z/\bar w) $$   


### Perspective Projection

$$ \mathbf{P} = \begin{bmatrix}  n & 0 & 0 & 0 \\\ 0 & n & 0 & 0\\\ 0 & 0 & n+f & -fn \\\0 & 0 & 1 & 0\end{bmatrix} $$   

The first, second, and fourth rows simply implement the perspective equation. The third row, as in the orthographic and viewport matrices, is designed to bring the *z*-coordinate “along for the ride” so that we can use it later for hidden surface removal. In the perspective projection, though, the addition of a non-constant denominator prevents us from actually preserving the value of *z*—it’s actually impossible to keep *z* from changing while getting *x* and *y* to do what we need them to do. Instead we’ve opted to keep *z* unchanged for points on the near or far planes.   

$$ \mathbf{P}\begin{bmatrix}  x \\\ y \\\ z \\\ 1 \end{bmatrix}  =  \begin{bmatrix}  x \\\ y \\\ z\frac{n+f}{n} - f \\\ \frac z n \end{bmatrix} \sim \begin{bmatrix}  \frac {nx}z \\\ \frac{ny}z \\\ n+f -\frac{fn}z \\\ 1 \end{bmatrix}  $$

![alt](/images/fundcg/7_persp1.png)    
![alt](/images/fundcg/7_persp2.png)    

As you can see, *x* and *y* are scaled and, more importantly, divided by *z*. Because both *n* and *z* (inside the view volume) are negative, there are no *“flips”* in *x* and *y*. Although it is not obvious (see the exercise at the end of the chapter), the transform also preserves the relative order of *z* values between $z = n$ and $z = f$, allowing us to do depth ordering after this matrix is applied. This will be important later when we do hidden surface elimination.   
Sometimes we will want to take the inverse of $\mathbf{P}$, for example to bring a screen coordinate plus $z$ back to the original space, as we might want to do for picking. The inverse is   

$$ \mathbf{P}^{-1} = \begin{bmatrix} \frac 1 n & 0 & 0 & 0 \\\ 0 & \frac 1 n & 0 & 0  \\\ 
0 & 0 & 0 & 1 \\\0 & 0 & -\frac 1 {fn} & \frac{n+f}{fn}\end{bmatrix}$$


or,   

$$ \mathbf{P}^{-1} = \begin{bmatrix} f & 0 & 0 & 0 \\\ 0 & f & 0 & 0  \\\ 
0 & 0 & 0 & fn \\\0 & 0 & -1  & n+f\end{bmatrix}$$   

$$ \mathbf{M}_{per} = \mathbf{M}_{ortho}\mathbf{P} $$   

So, the full set of matrices for perspective viewing is,   

$$ \mathbf{M}  = \mathbf{M}_{vp}\mathbf{M}_{ortho}\mathbf{P}\mathbf{M}_{cam} $$   

The resulting algorithm is,   

```python
compute M_vp
compute M_per
compute M_cam
M = M_vp * M_per * M_cam
for(line_segment (a_i, b_i))
    p = Ma_i
    q = Mb_i
    drawline(x_p/w_p, y_p/w_p, x_q/w_q, y_q/w_q)
```

$$ compute\;\mathbf{M}_{vp} \\\
compute\;\mathbf{M}_{per} \\\\ 
compute\; \mathbf{M}_{cam} \\\
\mathbf{M} = \mathbf{M}_{vp}\mathbf{M}_{per}\mathbf{M}_{cam} \\\
\mathbf{for}\; each \;line \;segment \;(\mathbf{a}_i, \mathbf{b}_i) \mathbf{do}: \\\
\quad    \mathbf{p} = \mathbf{Ma}_i \\\ 
\quad     \mathbf{q} = \mathbf{Mb}_i \\\
\quad      drawline(x_p/w_p, y_p/w_p, x_q/w_q, y_q/w_q)$$   

$$ \begin{array} 0 \mathbf{M}_{per} &=& \begin{bmatrix} \frac{2n}{r-l}  & 0 & \frac{l+r}{l-r} & 0 \\\ 0 & \frac{2n}{t-b} & \frac{b+t}{b-t} & 0 \\\ 0 & 0 & \frac{f+n}{n-f} & \frac{2fn}{f-n} \\ 0 & 0 & 1 &0\end{bmatrix} \\\ 
\mathbf{M}_{OpenGL} &=& \begin{bmatrix} \frac{2|n|}{r-l}  & 0 & \frac{r+l}{r-l} & 0 \\\ 0 & \frac{2|n|}{t-b} & \frac{t+b}{t-b} & 0 \\\ 0 & 0 & \frac{|n|+|f|}{|n|-|f|} & \frac{2|f||n|}{|n|-|f|} \\\ 0 & 0 & -1 &0\end{bmatrix}  \end{array}$$   


### Some Properties of the Perspective Transform

An important property of the perspective transform is that it takes lines to lines and planes to planes. In addition, it takes line segments in the view volume to line segments in the canonical volume.   

### Field-of-View

![alt](/images/fundcg/7_fov.png)    

