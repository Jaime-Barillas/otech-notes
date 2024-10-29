# Rendering - The Basics

+ Time vs Quality.
+ Fast & Low = Real-time.
+ Slow & High = Photorealistic rendering.
+ Fast & High = Impossible?

## Viewing & Projections

+ Need to transform a 3D space to a 2D image.
+ The **Viewing Transformation** encodes info:
  - Camera position.
  - Camera direction.
+ **Projection Transformations** convert 3D to 2D.
  - e.g. Perspective projection.
  - 4x4 matrix.

## Transformations

+ Homogeneous coordinates.
  - Scale and translation are easy.
+ Rotation is hard.
  - Gimbal lock.
    * Rotate one axis onto another and lose a degree of freedom.
  - Euler angles can't produce all possible 3D rotations.
+ Use either rotation matrices or quaternions.

### Rotation Matrices

+ To invert a rotation matrix, take its transpose.
+ Column vectors are orthogonal to row vectors.
+ To construct a rotation matrix:
  - Three orthogonal unit vectors.
  - Make them the rows of the rot. matrix.
  - The inverse can be constructed by using the vectors as the columns of the matrix.
  - Will rotate the orthogonal vectors onto the x, y, z axis.

## Coordinate Systems

+ World: Uses model transformation matrices to convert from model coords to
  world coords.
+ Eye: Produced by viewing transforms. (Camera pos, etc.)
  - Based on constructing a **Viewing Frustrum**.
  - If the frustum is symmetric about the viewing direction: **On Axis Projection**.
  - Otherwise **Off Axis Projection** (Good for head mounted displays and projectors.)
+ Window: Produced by projections.
  - Cube centered at origin (-1 to 1).
  - The projection matrix converts eye coords to this system.
  - Easy to scale to display device.

## The Hidden Surface Problem

+ Solved in hardware with a **depth buffer**.
  - Each pixel has a depth value.
  - Check the depth buffer against the depth of the current pixel.
  - May write a pixel many times.
    * Hardware implementation & simplicity makes up for inefficiency.
  - Limited resolution.
  - Multiple pixels in separate z values may map to same bucket in depth buffer.
  - Parallel projection: buckets have the same size $\Delta z = (\text{far} - \text{near}) / \text{Bin count}$
    * If all polygons ar more than $\Delta z$ apart, you're good.
    * Want smallest possible $\Delta z$.
  - Perspective projection: Buckets are different size.
    - Smaller bucket size closer to eye.
    - Larger bucket size further from eye.
      * Harder to detect differences in z.
    - Never set n to 0.
      * Bins become Infinitely large further away from the eye.
+ **Painter's algorithm**:
  - Sort back to front.
  - Draw back to front.
  - Sort in screen space aftre viewing and projection.
  - Can't sort polygons that overlap in z axis.
  - Sorting can be slow.
+ **BSP Trees**:
  - Sort polygons.
  - Order correct for _any viewpoint_.
  - Binary tree with polygon at each node.
  - Split on plane equation of each polygon.
  - Given eye pos, traverse the BSP tree.
    - If eye is in positive branch, negative polygons are further away.
      * vice versa.
  - No algorithm for optimal choice of root.
    * Create 5 at random and pick best one.
  - Polygons may cross plane $\rarr$ need to be split.
  - Best selection splits fewest polygons.
