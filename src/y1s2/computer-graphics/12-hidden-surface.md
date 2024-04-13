# Hidden Surface Removal

Three standard steps in rendering:
1. Viewing and Projection
2. Hidden Surface Removal <<<<- This one
3. Determining Surface Colour

## Motivation
+ Show front parts only
+ Avoid unnecessary processing

## Backface culling
Remove polygons where dot product of surface normal and view direction > (or <) 0.
+ for closed objects
+ remove faces early in pipeline
+ reduce polygon count by ~half
+ Sign depends on how view direction vector is defined:
  * v is view direction (from camera to polygon): > 0 (pos)
  * v is polygon to camera: < 0 (neg)

+ usually not sufficient due to partial overlaps
+ reliable only if no overlaps occur
+ order of objects depends on rendering sequence still.
+ Can reduce rendering time by up to half.

## Hidden surface elimination

+ Mostly done in hardware
+ 3 main algos
  * Painter's
  * Z buffer
  * BSP Tree

### Painter's Algorithm
+ sort scene's triangles from back to front
+ Draw back to front
+ Problems
  * cyclic overlaps
  * sort is O(n * log(n))
  * wasted time drawing invisible (covered) items.

### Z Buffer Algorithms
+ depth buffer
+ used in hardware
+ Post viewing transforms
+ z-depth is distance from viewer to point.
+ projection of 3D to 2D polygon
+ Efficient algos for computing pixels covered by a polygon exist
  * can interpolate z-values from projected verticies.
See math slide 20

projection is non-linear (looks logarithmic.)
+ Process:
  * z-buffer size of screen
  * all positions initialized to furthest point (far-plane f)
  * check current pos' z-buffer val
    - If current pos' z-val is smaller, draw and update z-buffer
+ Efficient?
  * May draw a pixel multiple times
  * No sorting
  * simplicity and hardware implementation make up for inefficiency
+ Points
  * memory required for z-buffer
  * No cyclic overlaps
+ Must scale z-values to fit within 32/64 bit ints/floats
  * For Ortho projection @ 32 bits:
    - B possible values = 2^32
    - ie. B bins with size (f - n)/B each (this is delta-z, surfaces delta-z or
      less close together give same z-buffer value)
    - Want polygons to be more than delta-z apart after transformations.
    - Otherwise: z-fighting.
  * Perspective:
    - See math in slide 34
    - closer to the eye = more precision
    - further from the eye = z-buffer value loses precision.
    - delta-z-world is bin size in _world coordinates_ = (z-world^2 * delta-z) / (f * n)
    - In model or world space, bins _aren't uniform size_
    - Never set n = 0, bins become extremely large far away from the eye.
    - High precision near the near-plane can be wasted if not used.

### BSP Tree

+ No way to sort polygon without splitting (think overlap or intersection)
+ use data structure that works for _all eye positions_
+ BSP Binary Space Partitioning.
+ Each polygon lies on plane: f(x, y, z) = 0
+ above formula can partition space into two parts: < 0 &  > 0
+ One side will be positive, other negative
+ If eye is on plus side, draw negative polygons first then current polygon
  then positiv polygons.
+ Just like a binary search tree!
+ Can split up polygons into smaller pieces if needed.
  * increases polygon count (dramatically?)
+ DOOM used this before most hardware had a z-buffer.
+ Traversing:
  * Draw polys behind first
  * Draw A
  * Draw polys in front.
  * **recursive**
+ Don't need to recalculate the tree (draw order is determined by plane
  equation of each poly and eye position)
+ Don't rely on graphics hardware.
+ No algorithm for optimal BSP tree
+ Nearly optimal algorithms exist

**Build method**
+ Assume no intersections
+ Choose a poly
+ add rest of polygons ordering with plane equation
  * test all verts w/ plane equation of current node.
  * if all pos add to pos subtree, otherwise neg subtree.
  * if some are pos, and some neg, split the poly
+ **Efficiency** depends on number of nodes.
  * every time we split, we get more nodes.
  * choice of root poly and order polys are added affect number of splits
  * Can choose root at random (gives sometimes good, sometimes bad results)
  * Instead choose a small number (5 emperically is good enough) and choose the
    one that splits the least.
