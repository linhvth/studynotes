---
layout: post
title:  Homography and Feature Detection
date:   2023-03-16
categories: computer-vision
permalink: /computer-vision/eng301-cv/lecture-6/
---

### Table of Contents

1. [Image Homography](#image-homography)
    - [2D Transformations](#2d-transformations)
    - [3x3-Matrix Representation of 2D Transformations](#3x3-matrix-representation-of-2d-transformations)
    - [Affine Transformation](#affine-transformation)
    - [Projective Transformation](#projective-transformation)
2. [Feature Detection](#feature-detection)

# Image Homography

## 2D Transformations

This part aims to review 4 basic 2D transformation: [Translation](#translation), [Scaling](#scaling), [Rotation](#rotation), and [Shear](#shear). The concept of [Composite Transformation](#composite-transformations) will be also introduced.

### Translation

To move an object to a different position.

$$\mathbf{P}\prime = \begin{bmatrix} x\prime \cr y\prime \end{bmatrix} = \begin{bmatrix} x \cr y\end{bmatrix} + \begin{bmatrix} t_x\cr t_y\end{bmatrix} = \mathbf{P} + \mathbf{T}$$

### Scaling

To change the size of an object. If there is an **uniform** scaling, in which each component is multiplied by **the same scalar**, which means $$s_x = s_y$$

$$\mathbf{P}\prime 
= \begin{bmatrix} x\prime \cr y\prime \end{bmatrix} 
= \begin{bmatrix} s_x x \cr s_y y\end{bmatrix} 
=  \begin{bmatrix} s_x & 0 \cr 0 & s_y\end{bmatrix} \begin{bmatrix} x \cr y\end{bmatrix}
$$

### Rotation

To rotate the object at particular angle $$\theta$$ from its origin.

$$ \mathbf{P}\prime 
= \begin{bmatrix} x\prime \cr y\prime \end{bmatrix} 
= \begin{bmatrix} x\cos{\theta} - y\sin{\theta} \cr x\sin{\theta} + y\cos{\theta} \end{bmatrix} 
= \begin{bmatrix} \cos{\theta} & -\sin{\theta} \cr \sin{\theta} & \cos{\theta} \end{bmatrix} \begin{bmatrix} x \cr y\end{bmatrix}
= \mathbf{R} \mathbf{P}
$$

<details>
<summary>Detailed calculation for rotation</summary>

 <p align="center">
 	<img src="{{ site.url }}/assets/images/eng301-cv/l6-2d-rotation.png" alt="drawing" width="500" class="center"/>
 </p>

$$x = r\cos{\phi} \\ y = r\sin{\phi} 
\\ x\prime = r\cos{(\phi + \theta)} 
= r\cos{\phi}\cos{\theta} - r\sin{\phi}\sin{\theta}
= x\cos{\theta} - y\sin{\theta}
\\ y\prime = r\sin{(\phi + \theta)} 
= r\cos{\phi}\sin{\theta} + r\sin{\phi}\cos{\theta}
= x\sin{\theta} + y\cos{\theta}
$$

</details>

### Shear

Shearing, or Skewing, is a transformation to slant the shape of an object.

$$\begin{bmatrix} x\prime \cr y\prime \end{bmatrix}
= \begin{bmatrix} 1 & h_x \cr h_y & 1 \end{bmatrix} \begin{bmatrix} x \cr y \end{bmatrix}$$

### Composite Transformations

Composing Transformation is the process of applying several transformation in succession to form one overall transformation. Below is some common classifications of 2D Transformations:

- Translation
- Rigid (Euclidean) (rotation + translation)
- Similarity (uniform scaling + Rigid)
- [Affine](#affine-transformation) (Similarity + shearing)
- Projective (Affine + projective wraps)

 <p align="center">
    <img src="{{ site.url }}/assets/images/eng301-cv/l6-class-2d-trans.png" alt="drawing" width="500" class="center"/>
 </p>

  <p align="center">
    <img src="{{ site.url }}/assets/images/eng301-cv/l6-diff-trans-wrap-cmu.png" alt="drawing" width="500" class="center"/>
 </p>

**Important Note:** Matrix Multiplication is associative, but not commutative. The 2 composing transformation $$\mathbf{A}\mathbf{B} \neq \mathbf{B}\mathbf{A}$$ (some cases $$\mathbf{A}\mathbf{B} = \mathbf{B}\mathbf{A}$$ such as performing rotation twice).

## 3x3-Matrix Representation of 2D Transformations

<details>
<summary>Why do we want to use 3x3 matrix representation?</summary>

- Translation is non-linear transformation while Rotation, Scaling, and Shearing are linear transformations. Thus, we need a way to manipulate it => Homogeneous Coordinates. 
- Note that, if we use 2x2 transformation matrices, we have to multiply multiple matrices together, which leads to the confusion since different order leads to different result matrix. For example, Affine Matrix has a fixed order of matrix transformations, we cannot screw it. Thus, using 3x3 matrices help us to "pre-multiply" all transformation matrices together, and we can perform all transformations using matrix/vector multiplications.

</details>

### Translation

$$\begin{bmatrix} x\prime \cr y\prime \cr 1 \end{bmatrix}
= \begin{bmatrix} 1 & 0 & t_x \cr 0&1&t_y \cr 0&0&1 \end{bmatrix} \begin{bmatrix} x\cr y\cr 1 \end{bmatrix}$$

### Rotation

$$\begin{bmatrix} x\prime \cr y\prime \cr 1 \end{bmatrix} 
= \begin{bmatrix} x\cos{\theta} - y\sin{\theta} \cr x\sin{\theta} + y\cos{\theta} \cr 1 \end{bmatrix} 
= \begin{bmatrix} \cos{\theta} & -\sin{\theta} & 0 \cr \sin{\theta} & \cos{\theta} & 0 \cr 0&0&1 \end{bmatrix} \begin{bmatrix} x \cr y\ \cr 1 \end{bmatrix}
$$

### Scaling

$$\begin{bmatrix} x\prime \cr y\prime \cr 1 \end{bmatrix} 
= \begin{bmatrix} s_x x \cr s_y y \cr 1 \end{bmatrix} 
=  \begin{bmatrix} s_x & 0 & 0 \cr 0 & s_y & 0 \cr 0&0&1 \end{bmatrix} \begin{bmatrix} x \cr y \cr 1 \end{bmatrix}
$$

## Affine Transformation

- From Mathematics perspectives, let's review Linear Algebra (Linear Transformation and Affine Transformation).
- Are combinations of arbitrary (4-DOF) **linear** transformations and translations.
- Any 2D affine transformation can be decomposed into a rotation, followed by a scaling, followed by a shearing, and followed by a translation. 
- Affine matrix = translation x shearing x scaling x rotation.

Denote the Affine matrix as $$\mathbf{A}$$, assume we have uniform scaling transformation. We have:

$$
\begin{align*}
\begin{bmatrix} x\prime \cr y\prime \cr 1 \end{bmatrix}  
&= \begin{bmatrix} 1 & 0 & t_x \cr 0&1&t_y \cr 0&0&1 \end{bmatrix}
\begin{bmatrix} 1 & h_x & 0 \cr h_y&1&0 \cr 0&0&1 \end{bmatrix}
\begin{bmatrix} s & 0 & 0 \cr 0 & s & 0 \cr 0&0&1 \end{bmatrix}
\begin{bmatrix} \cos{\theta} & -\sin{\theta} & 0 \cr \sin{\theta} & \cos{\theta} & 0 \cr 0&0&1 \end{bmatrix}
\begin{bmatrix} x \cr y \cr 1 \end{bmatrix} \\
&= \begin{bmatrix} a&b&c \cr d&e&f \cr 0&0&1 \end{bmatrix} 
\begin{bmatrix} x \cr y \cr 1 \end{bmatrix} \\
&= \mathbf{A} \begin{bmatrix} x \cr y \cr 1 \end{bmatrix}
\end{align*}
$$

The Affine matrix $$\mathbf{A}$$ has the degree of freedom of 6.

## Projective Transformation


# Feature Detection

---

#### References
- Fulbright University Vietnam - ENG 301 Computer Vision by Dr. Duong Phung
- [The Ohio State University - Introduction to Computer Graphics - Transformation Review](https://web.cse.ohio-state.edu/~shen.94/681/Site/Slides_files/transformation_review.pdf)
- [CMU - 16-385 Computer Vision](https://www.cs.cmu.edu/~16385/)
