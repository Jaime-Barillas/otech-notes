# Advanced Curves

## Bezier Curves
Made up of 3 control points in 2D

+ Good local control properties
+ Changes affect at most two segments
+ Bounded by **convex hull**: smallest polygon that contains all control
  points.
+ **variation diminishing property**: A line intersects the curve _no more_
  (ie. this many or less) times than it intersects the lines connecting the
  control points.

+ **affine invariant**: Can translate, rotate, scale, and skew the control
  points and the curve will be transformed in the same way.
+ ergo, transform the control points to transform the curve.
+ Bezier curves are symmetric, reverse the control points and you get the same
  curve.

+ Usually a shared control point scheme is used where the last control point of
  one piece is the first control point of the next.
  * Guarantees C^0 continuity, the derivatives are controlled by other control
    points.
  * To get G^1: The 3 control points at a join must be collinear.
  * To get C^1: The 3 control points must be, additionally, of equal distance
    from the center control point.

+ Main disadvantage: Only guarantee C^0 continuity.
  * With additional constraints above: C^1 can be reached.

## Cubic Bezier Curves
4 Control points.

+ Interpolates the first and fourth.
+ approximating the second and third.
+ derivative at start (u = 0): value is 3 times the vector from the first to
  the second control point.
+ derivative at the end (u = 1): value is 3 times the vector from the third to
  fourth control point.
+ position of curve is controlled by first and fourth control point.
+ shape of curve is controlled by the second and third.

See math slide 18

p0 = a0
p1 = a0 + (1/3)a1
p2 = a0 + (2/3)a1 + (1/3)a2
p3 = a0 + a1 + a2 + a3

**Cubic Bezier Constraint matrix (C):**

1  0   0   0
1  1/3 0   0
1  2/3 1/3 0
1  1   1   1

**Blending Matrix (B):**

Inverse of the Constraint matrix.

**Bernstien Polynomials**

Multiply _out_ uBp -> get bernstien polynomials.
+ They are all cubic.
+ Labelled with subscripts:
  * b\_0,3
  * b\_1,3
  * b\_2,3
  * b\_3,3

## Evaluating a Point on a Cubic Bezier Curve

+ p(t) = (p0 * b\_0,3) + (p1 * b\_1,3) + (p2 * b\_2,3) + (p3 * b\_3,3)
+ See slides(22) for definitions of functions b\_0,3 to b\_3,3

## Bezier Surfaces/Patches
+ application of bezier curves to two parametric directions.
+ (m * n) Control points.
+ P(u, v) = sumj-to-m( sumi-to-n( p\_j,i * B\_j,m * v * B\_i,n * u ) )

## How to render (freeform surfaces)
freeform surface spec (presumably bezier surfaces above.):
+ points on surface by evaluating the sums above.
+ order of points, through param order.
+ Can extract polygon mesh
  * choose param stepping size for u & v
  * compute points for each step
  * create polygon mesh using inherent order
+ Can be as detailed as needed.
