# Transformations

In Euler angles rotations are about a single axis at a time.
different versions of Euler angles depends on the order of the axis (rotations?)
none of those orders work in general.
Euler proved you can't specify arbitrary rotation in 3D with just 3 numbers.

Euler angles don't work sometimes:
90deg rotation about X-axis will rotate Y-axis to the Z-axis. Can't
use the original Y-axis anymore. This is called a Gimbal lock.

A rotation matrix rotates its own rows onto the X, y, z axis.

The inverse of a rotation matrix is its Transpose.

R^t will rotate the different axis onto its rows.

An easy way to construct a rotation matrix to rotate
3 orthogonal vectors (u, v, w):
+ Rotate onto x, y, z axis with R
+ For example, the rotate about some u vector: Use basic x-axis rotation matrix to rotate about u
+ Rotate axes back onto (u, v, w) vectors with R^t
+ NOTE: (u, v , w) must be unit vectors! normalize first!
Construct R as:
+ first row is u vector
+ second row is v vector
+ third row is w vector
+ its 4x4 matrix.

Rotate about arbitrary axis
======
+ rotate axis a onto the z-axis.
+ perform rotation about z-axis.
+ rotate back to original fram of reference with transpose of original
  rotation.

+ a should be normalized.
+ a is the third row of the rotation matrix
+ But what about the other two rows, they need orthogonal vectors.
  for an orthogonal vector u:
  * make a vector t that is equal to a
  * change the smallest component to 1
  * take the cross-product of t and a
  * normalize it.
  * To get the final vector v, take the cross-product of u and a.
  * The Rotation matrix has **u as the first row, v as the second row, a as the
    third row**. It's also a 4x4 matrix.

It is possible that transforming a surface and then its normal will result
in a normal that is not perpendicular to the transformed surface!
Affine transforms including scale do not preserve angles between lines.

Transforming the Normal
====
the relationship between the normal and a vector tangent to the surface (call
it t):

(matrix-vector multiplication) $n^T \cdot t = 0$

For transoformation M:
$n'^T \cdot (M \cdot t) = 0$    <-- What is n'?

See the math in the slides

**The matrix to transform the normal is the transpose of the inverse of the
original transformation matrix**:
$n' = (M^{-1})^T \cdot n$

**Note:** n' may no longer be unit length so normalize to be safe.

**Note:** If M contains only translations and rotations then we can just use M
instead of the above math. (Rotations and translations are considered rigid body
transformations which preserve the normal vector.) (The inverse of a rotation
matrix is just its transpose so we are essentially transposing the M twice in this
case.)

Careful with reflections in 3D! they can change the handed-ness of the coord system.
An odd number of 1's => change in handedness.

Decomposing Transformations
====
All transforms can be broken down into a translation, 2 rotations, and a scale.
+ The translation is on always placed on the left side in a multiply:
  `translation-matrix * partially-broken-down-matrix = original-transform-matrix`
+ If the partially-broken-down-matrix that remains is symmetric:
  eigen value decomposition can be done to get a RSR^t (rotation-scale-rotationTranspose)
  matrix.
  * S is a diagonal matrix with the entries being the eigen values of the partially-broken-down-matrix.
  * R gets its columns from the eigen vectors.
+ If not symmetric:
  Use SVD (single value decomposition.) Not taught it seems.
