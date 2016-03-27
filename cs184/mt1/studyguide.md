---
---

# Sampling

_Nyquist frequency_: the minimum rate at which a signal can be sampled without introducing aliasing. Equal to twice the highest frequency in the signal.

## Antialiasing

Remove frequencies above the Nyquist frequency before sampling. Done by filtering before sampling.

## Triangle Equations

### Line equation

$L(x, y) = Ax + By + C$

* On line: $L(x, y) = 0$
* On right: $L(x, y) > 0$
* On left: $L(x, y) < 0$

A point is inside the triangle if line tests for all lines (going clockwise) is greater than 0.


# Transforms

## Basic Transformations

### Scaling

$$\begin{bmatrix}
s & 0 & 0 \\
0 & s & 0 \\
0 & 0 & 1
\end{bmatrix}$$

### Rotation

$$\begin{bmatrix}
\cos(\theta) & -\sin(\theta) & 0 \\
\sin(\theta) & \cos(\theta) & 0 \\
0 & 0 & 1
\end{bmatrix}$$

### Translation

$$\begin{bmatrix}
1 & 0 & t_x \\
0 & 1 & t_y \\
0 & 0 & 1
\end{bmatrix}$$

### Shear



## Homogeneous Coordinates

Points are defined as $(x, y, 1)$ and vectors by $(x, y, 0)$. Points and vectors don't reside on the same $xy$ plane.

## Affine Transformations

Consist of linear map and a translation.

## Camera Transformations

Given eye point $e$, up vector $u$, view direction $v$, where $u$ and $v$ are orthonormal, find right vector $r = v \times u$

Transform camera $(e, r, u, v)$ to standard camera located at the origin, looking down the negative $z$-axis, where the up vector is the $y$-axis.

## Projective Transform

Consider center of projection at <0 0 0> and image plane at $z = d$, then <x y z> -> <x*d/z y*d/z d>.

## Perspective Projection

* fovy: vertical angular field of view
* aspect: width/height of field of view
* near: depth of near clipping plane
* far: depth of far clipping plane

* top = near * tan(fovy)
* bottom = -top
* right = top * aspect
* left = -right

Convert from camera coordinates to normalized device coordinates (NDC), where (left, bottom, -near) -> (-1, -1, -1) and (right, top, -far) -> (1, 1, 1). Linear transformation in homogeneous coordinates.

Transform matrix:

[[near/right 0 0 0]
 [0 near/top 0 0]
 [0 0 -(far+near)/(far-near) -2(far*near)/(far-near)]
 [0 0 -1 0]]

## Summary

* Object coordinates: Apply modeling transforms
* World (scene) coordinates: Apply viewing transform
* Camera (eye) coordinates: Apply perspective transform and homogeneous division
* Normalized device coordinates: Apply 2D screen transform
* Screen coordinates

# Texture Mapping

## Barycentric Coordinates

(\alpha, \beta, \gamma) have to add up to 1.

$(x, y) = \alpha A + \beta B + \gamma C$, where A, B, and C are values at vertices of the triangle.

Area transformation:

https://www.dropbox.com/s/u8gzm3y17ij0o2q/Screenshot%202016-03-12%2013.25.45.png?dl=0

Proportional distances

https://www.dropbox.com/s/5li8fstc0mzf3c6/Screenshot%202016-03-12%2013.26.06.png?dl=0

Coordinate formula

https://www.dropbox.com/s/nzq0ptlz9quaiki/Screenshot%202016-03-12%2013.42.23.png?dl=0


# Graphics Pipeline


# Intro to Geometry


# Splines, Curves, and Surfaces


# Geometry Processing


# Accelerating Ray Tracing


# Radiometry and Photometry

## Radiant Flux



## Radiant Intensity

Power per unit solid angle emitted from a point light source.

## Irradiance

Power per unit area (W/m^2).

Lambert's cosine law

Irradiance falloff follows inverse square law



## Radiance

Power per unit area per unit solid angle (W/sr/m^2).



# Monte Carlo Integration


# Reflection and Materials

## BRDF

* Non-negative (always returns a positive value for any input ray)
* Respects linearity.
* Reciprocity principle: $f_r(w_i \rightarrow w_r) = f_r(w_r \rightarrow w_i)$
* Energy conservation

# Global Illumination and Path Tracing


# Advanced Sampling and Rendering


# Useful formulas

Fourier transform: $F(\omega) = \int_{-\infty}^{\infty} f(x) e^{-2\pi i \omega x} \,dx$

Inverse transform: $f(x) = \int_{-\infty}^{\infty} F(\omega) e^{2\pi i \omega x} \,d\omega$

Euler's formula: $e^{ix} = \cos x + i\sin x$
