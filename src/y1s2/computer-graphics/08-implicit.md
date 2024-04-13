# Implicit and Parametric Equations

## Implicit Representations
+ f(x,y) = 0
+ points that satisfy the above are _on the curve_.

## Implicit Line
+ direction of line: d = p1 -p0
+ every point on the line must be parallel to d
+ every point has perpendicular vector: n = (d.y, -d.x)
+ See that d dot n = 0

So what is f(x, y)?
+ given p0 = (x0, y0) and p1 = (x1, y1)
  * n = (y1 - y0, -(x1 - x0))
  * f(q) = n dot (q - p0) = 0  Where q is any point _on_ the line.

# Implicit Circle
(x - xcenter)^2 + (y - ycenter)^2 -r^2 = 0  
or  
|| p - pcenter ||^2 = r^2

# 3D

Implicit reprsentations for spheres:
(x - xcenter)^2 + (y - ycenter)^2 + (z - zcenter)^2 - r^2 = 0

# Display
To display these we need to know their normal vectors.
For implicit surfaces it is the gradient:

gradient( f ) = [ die( f, x) die( f, y) die( f, z) ]

Follow the gradient to the intersection point of the surface.
Once a point is found, use the gradients and tangents to find
more points on the surface. Not the easiest.

# Parametric Representations
Easier to draw than Implicit representations.

+ Have a parameter (t) that varies along the curve.
+ Use a function of that parameter to generate points along the curve.

Example of a Circle parametric equation:

x = r * cos(t)
y = r * sin(t)
for 0 <= t <= 2pi

**Parameterization is NOT unique**

> Arc length parameterization
> + not common
> + | derivative (f, s) |^2 = c   For some constant c.
> + s is distance along the curve.

### Unit Parameterization
Use 0 <= t <= 1

For circle:

x = r * cos(2pi * t)
y = r * sin(2pi * t)

## 3D

(x, y, z) = p
f(u, v) = (x, y, z) = p

We use two parameters to trace out the surface.

A 3D object's surface has n-1 dimensions (i.e. 2D)
Therefore, we need only n-1 parameters to draw its surface.

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
+ C^n continuous: Its nth derivative is continuous.
  * Affects: shape and speed.
  * for curves and _particularly_ where the connect.

C^0 continuous: a corner
The curve segments match up, there are no gaps but it might not be smooth.

C^1: first derivative is equal at the join points.
C^n: nth derivative is equal at join points.

### Parametric Representations Continuity

Derivatives may have the same direction, but different magnitude due
to the parameterization.

This is **Geometric Continuity**:
The curve segments match up and their tangents (nth derivatives) have the
same direction but different magnitudes.

Two curves are G^1 continuous if:
f'1 = k * f'2    k is a scalar constant.
