# Implicit and Parametric Equations

## Implicit Representations
+ $f(x, y) = 0$
+ points that satisfy the above are _on the curve_.

## Implicit Line
+ direction of line: $\vec{d} = p_1 - p_0$
+ every point on the line must be parallel to $\vec{d}$
+ every point has perpendicular vector: $\vec{n} = (\vec{d}.y, -\vec{d}.x)$
+ See that $\vec{d} \cdot \vec{n} = \vec{0}$

So what is $f(x, y)$?
+ given $p_0 = (x_0, y_0)$ and $p_1 = (x_1, y_1)$
  * $\vec{n} = (y_1 - y_0, -(x_1 - x_0))$
  * $f(q) = \vec{n} \cdot (q - p_0) = 0$  Where $q$ is any point _on_ the line.

# Implicit Circle
$(x - x_{\text{center}})^2 + (y - y_{\text{center}})^2 -r^2 = 0$  
or  
$||p - p_{\text{center}}||^2 = r^2$

# 3D

Implicit reprsentations for spheres:
$(x - x_{\text{center}})^2 + (y - y_{\text{center}})^2 + (z - z_{\text{center}})^2 - r^2 = 0$

# Display
To display these we need to know their normal vectors.
For implicit surfaces it is the gradient:

$\nabla f = (\frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z})$

Follow the gradient to the intersection point of the surface.
Once a point is found, use the gradients and tangents to find
more points on the surface. Not the easiest.

# Parametric Representations
Easier to draw than Implicit representations.

+ Have a parameter ($t$) that varies along the curve.
+ Use a function of that parameter to generate points along the curve.

Example of a Circle parametric equation:

$x = r \cdot cos(t)$  
$y = r \cdot sin(t)$  
for $0 <= t <= 2\pi$

**Parameterization is NOT unique**

> Arc length parameterization
> + not common
> + $|\frac{df}{ds}|^2 = c$   For some constant c.
> + $s$ is distance along the curve.

### Unit Parameterization
Use $0 <= t <= 1$

For circle:

$x = r \cdot cos(2pi * t)$  
$y = r \cdot sin(2pi * t)$

## 3D

$(x, y, z) = p$  
$f(u, v) = (x, y, z) = p$

We use two parameters to trace out the surface.

A 3D object's surface has $n-1$ dimensions (i.e. 2D)
Therefore, we need only $n-1$ parameters to draw its surface.

# Piecewise Curves

+ For simple shapes, the above parametric is fine.
+ For complex shapes it's not.
+ Use a piecewise representation for more complex curves.
  * disadvantage: Needs more functions.
  * advantage: simpler functions.
  * advantage: easier to work with/program.
  * advantage: You can get any curve w/ enough pieces.

## Piecewise Curve Continuity
"How the pieces fit together."
**Parametric Continuity**:
+ $C^n$ continuous: Its nth derivative is continuous.
  * Affects: shape and speed.
  * for curves and _particularly_ where the connect.

$C^0$ continuous: a corner
The curve segments match up, there are no gaps but it might not be smooth.

$C^1$: first derivative is equal at the join points.
$C^n$: nth derivative is equal at join points.

### Parametric Representations Continuity

Derivatives may have the same direction, but different magnitude due
to the parameterization.

This is **Geometric Continuity**:
The curve segments match up and their tangents (nth derivatives) have the
same direction but different magnitudes.

Two curves are G^1 continuous if:
$f^{(1)} = k * f^{(2)}$, where $k$ is a scalar constant.
