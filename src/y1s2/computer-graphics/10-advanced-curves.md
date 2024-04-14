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
  * Guarantees $C^0$ continuity, the derivatives are controlled by other control
    points.
  * To get $G^1$: The 3 control points at a join must be collinear.
  * To get $C^1$: The 3 control points must be, additionally, of equal distance
    from the center control point.

+ Main disadvantage: Only guarantee $C^0$ continuity.
  * With additional constraints above: $C^1$ can be reached.

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

$p0 = a_0$  
$p1 = a_0 + (\frac{1}{3})a_1$  
$p2 = a_0 + (\frac{2}{3})a_1 + (\frac{1}{3})a_2$  
$p3 = a_0 + a_1 + a_2 + a_3$

**Cubic Bezier Constraint matrix (C):**

$$
\begin{bmatrix}
1&  0&           0&           0& \\
1&  \frac{1}{3}& 0&           0& \\
1&  \frac{2}{3}& \frac{1}{3}& 0& \\
1&  1&           1&           1&
\end{bmatrix}
$$

**Blending Matrix (B):**

Inverse of the Constraint matrix.

**Bernstien Polynomials**

Multiply _out_ uBp -> get bernstien polynomials.
+ They are all cubic.
+ Labelled with subscripts:
  * $b_{0,3}$
  * $b_{1,3}$
  * $b_{2,3}$
  * $b_{3,3}$

## Evaluating a Point on a Cubic Bezier Curve

+ $p(t) = (p0 \cdot b_{0,3}) + (p1 \cdot b_{1,3}) + (p2 \cdot b_{2,3}) + (p3 \cdot b_{3,3})$
+ See slides(22) for definitions of functions $b_{0,3}$ to $b_{3,3}$

## Bezier Surfaces/Patches
+ application of bezier curves to two parametric directions.
+ $m \cdot n$ Control points.
+ $P(u, v) = \overset{m}{\underset{j}{\sum}}(\overset{n}{\underset{i}{\sum}}( p_{j,i} \cdot B_{j,m} \cdot v \cdot B_{i,n} \cdot u ))$

## How to render (freeform surfaces)
freeform surface spec (presumably bezier surfaces above.):
+ points on surface by evaluating the sums above.
+ order of points, through param order.
+ Can extract polygon mesh
  * choose param stepping size for u & v
  * compute points for each step
  * create polygon mesh using inherent order
+ Can be as detailed as needed.
