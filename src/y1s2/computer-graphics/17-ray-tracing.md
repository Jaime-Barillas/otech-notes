# Ray Tracing

+ Global illumination model  
  Two techniques:
  - Ray Tracing
  - Radiosity

## Radiosity

+ Not used much anymore.
+ Ideas incorporated into other techniques.

## Properties of Light

+ Law of conservation of energy.
+ CG light is special case
  - light energy may convert to other form.
  - other forms may convert to light energy.
+ Light is a wave (see light model lecture)
+ Light is a particle (carries amount of energy, used in raytracing)
+ CG light follows straight paths.

## Towards Ray Tracing

+ Phong is coarse approximation.
+ Goal: Look at real physical conditions.
  - Use particle model of light.
  - Derive reflected amount of light from:
    * Incoming light from different directions.
    * More realistically modeled reflection properties.
    * Own emitted light.

## Basic Ideas - Rays in Ray Tracing

+ One ray per pixel.
+ Rays originate at the pixel, directed towards the eye.
+ How to determine the pixel colour?
+ Reverse the ray, trace it into the scene.
  - The ray starts at the pixel and goes into the scene.
  - It hits an object or goes into the background
+ Compute the colour of the object where the ray hits it.
  - Don't forget the light contributions from other objects in the scene.

So:

+ Trace a ray from each intersection point to each of the light sources.
  - If a clear path to the light, compute the diffuse and specular light
    produced by that light.
  - If there isn't a clear path, that object is in shadow from the light source
    and only the ambient component of light is used.
+ The ray can also reflect off of surfaces that it hits.
+ Trace the ray to any object it may intersect. Compute that colour as the
  reflected colour for the original object.
  - If the object is transparent, trace the ray through the object to find the
    colour on the other side (refract the ray). Then follow the ray to its next
    intersection and compute the colour there.
+ This process can be recursive. Usually a couple levels until stopping.

## The Math

TODO

## Adapted Illumination Model

See slide 8.
I = ambient + sum(diffuse + specular) + reflected-ray-values + refracted-ray-values.

## Forward Ray Tracing

We trace from eye to object (through pixels) to light source.
**Forward ray tracing** traces rays from light source to object (eventually to
eye).

+ Lots of work (rays) for little return.
+ Some of the rays will reflect in such a way that they don't intersect with
  the pixels of the screen and the eye.

## Backwards Ray Tracing

+ What we do.
+ From the eye to (eventually) the light source.
+ "Do work where it matters".

## Efficiency of Backwards Ray Tracing

+ Each of the original rays can generate more rays.
+ The final result of the original ray is the pixel colour.
+ Can be modeled as a tree of rays, pixels are the root.
+ Each intersection is a node in the tree. The generated rays lead to the
  children.
+ Called a **Ray Tree**.
+ Contribution of child rays to colour lessens each intersection.
+ The Ray Tree can quickly grow large.
+ Ray Tracing can become expensive to compute.
+ We want to control number of rays.
+ Intersection computations must be efficient.

## Stopping Conditions for Ray Tracing

+ Two options
  1. Stop when the tree reaches a maximum depth.
  2. Stop when the contribution to colour goes below a certain level.
+ Usually, both are used.

## Finding Intersections

+ When tracing rays, we want the closest intersection.
+ Other objects are hidden by the closest, so only the closest matter to the pixel colour.
+ Naive approach is to test every object in the scene.

## Coordinate Systems

First we need to put everything in a common coord system before generating
rays.

### Eye Coordinates

When using eye coords, transform the models so they are in the eye coordinate
system. Only needs to be done once, so not a performance concern.

### Model Coordinates

Place the eye and display screen in model coordinates. Needs to be applied to
each of the original rays. Still not an efficiency concern.

## Intersections

The most expensive operation. The mathematical representation of a ray:
$p(t) = e + t\vec{d}$

$e$ is the starting point of the ray. $\vec{d}$ is the ray direction. $t$ must
always be greater than 0, otherwise, any intersections will be at or behind the
eye.

## Sphere Intersection

+ One of the easiest objects to intersect.
+ $f(e + t\vec{d}) = 0$ or in vector from:
+ $(p - c)^2 - R^2 = 0$
  - $c$ is the center point and $R$ is the sphere radius.
  - After substitution and simplification, results in a quadratic function in
    $t$. Solve with the quadratic formula.
+ Want the smallest positive of two roots. (closest intersection in _front_.)
+ If both roots are complex, ray does not hit sphere.
  - Use discriminant to check.

# Ray Tracing Part 2

Review of **raycasting**:
+ Light rays, for each intersection -> compute local illumination using Phong
  model.
+ Shadow rays, for testing if intersection points have line-of-sight to light
  source.
+ True raytracing: Recursive, follow reflected and refracted rays.

## General Intersections

General polygon intersection.
+ Two steps
  - Where does the ray intersect the polygon's plane? (Is it parallel or not?)
  - Is the intersection point inside or outside the polygon?
+ Polygon plane eq'n:
  $(p - p_1) \cdot \vec{n} = 0$,   for any point p on the plane of the polygon.  
  $p_1$ is a polygon vertex, $n$ is the normal for the polygon.
+ $(e + t\vec{d} - p_1) \cdot \vec{n} = 0$,   $p$ is $e + t\vec{d}$.  
  After rearranging: $t = \frac{((p_1 - e) * \vec{n})}{(\vec{d} * \vec{n})}$
+ If $(\vec{d} \cdot \vec{n})$ is 0, the ray is parallel to the plane.
+ Project the polygon and intersection point to one of the coordinate planes.
+ Draw a line from the intersection point to infinity, Count the number of times
  it intersects a polygon edge.
  - Odd -> inside polygon
  - Even -> outside polygon

So which line to infinity should we use? (This selects the coordinate plane
to project onto)
+ An easy choice is the x-axis.
  - Intersection calculation is easy, check y values only.
+ If one end point of polygon edge is above the line (x-axis in this case) and
  the other below, we have an intersection.
+ If the projection results in a horizontal line, choose one of the other
  planes.

+ More efficient computations in the case of triangles.
+ Efficiency is a major concern in raytracing.
+ Key observation: _Most objects will not be hit by a particular ray_.
+ Want: Compute **non-intersections** as quickly as possible.
+ Sol'n: Bounding volumes.

## Bonuding Volumes

+ Enclose complex objects in a simpler shape.
+ Test intersection against the simpler shape first (e.g. a sphere).
+ Use a sphere, axis-aligned bounding box, rectangular prism.
+ Prefer:
  - Bounding volume closely contains the object.
    * Reduce possibility of false positives.
  - Add to modeling heirarchy for heirarchy of bounding volumes.

## Accelerating RayTracing

Parallelizeable - raytracing is emberassingly parallel.
Space Partitioning - Bounding Volume heirarchies, BSP, uniform subdivision trees
(Octrees)

## Space Partitioning

+ Want closest intersection to start of ray
+ More likely desired intersection occurs near start of the ray.
+ Intersection will not occur behind the ray or in a direction the array doesn't go.
+ The ray passes through a small portion of space, don't look elsewhere.
+ So we need a way of organizing the objects in the scene so we only look at the
  ones where the ray will pass and ignore the rest of them.

+ Want to test objects in smart order, most likely first, but make sure you don't
  miss closer ones.
+ Inefficient to sort objects by distance since they would need to be sorted
  for _each_ ray.
+ A Sol'n:
  - Build grid on top of scene.
  - Assign objects to the grid cells they are in.
  - Each cell should hold a small number of objects (ideally 3 - 5)
  - Start intersection testing at cell where ray starts
  - Find first cell the ray hits
  - Objects in that cell are closest to ray origin, test them.
  - If no intersection, move on to next cell.
  - Worst Case (n x n x n 3D grid): Have to check n * sqrt(3) cells.
  - For evenly distributed objects: Speed up on order of 1/n^2 (if n = 10 -> 100 times faster)
  - In practice, objects are rarely evenly distributed.
  - Problems:
    * As n is increased data structure grows by n^3
    * As cell size gets smaller, an object may appear in multiple cells causing
      multiple intersection calculations for that model.
    * Many cells will be empty, computing next cell to visit takes time so we
      waste efficiency on empty cells.
    * Lower limit on cell size exists.

## Adaptive Space Partitioning

+ We want cells to be as large as possible so we can chew through them quickly.
+ Use an _adaptive grid with cells of varying size_.
+ May improve performance by reducing the number of empty cells.

## Recursive (Normal) RayTracing

### Hard Shadows

+ Use a shadow feeler for each light source.
  - A ray cast from each intersected object to the light source.
  - If feeler intersects another object before hitting the light source, the
    object is in shadow.
+ Only for point light sources and infinitely far away ones (directional.)

### Reflections

+ Need reflection direction.
+ Perfect mirror reflection.
+ $\vec{r} = \vec{d} - 2(\vec{d} \cdot \vec{n})\vec{n}$
+ Apply ray-tracing computation recursively to find pixel colour.
+ Contributes to _specular light_ component.
+ Each recursion (extra reflection ray), contributes less and less.

### Refraction

+ **Snell's law**: $\eta\sin{\theta} = \eta_t\sin{\phi}$
+ $\eta$ and $\eta_t$ are the **indicies of refraction**.
  - These are known
+ $\theta$ is also known, we solve for $\phi$
+ Snell's law gives us the angle but not the direction vector $\vec{t}$.
+ Use $t = \vec{b}\sin{\phi} - \vec{n}\cos{\phi}$
  - **Basis vectors** $\vec{b}$ and $\vec{n}$ needed.
  - $\vec{n}$ is normal.
  - $\vec{b}$ is parallel to medium's surface.
  - Incident vector $\vec{d}$ can be used to find $\vec{b}$:
  - $t = \frac{\eta}{\eta_t}(\vec{d} - \vec{n}(\vec{d} \cdot \vec{n})) - \vec{n} \sqrt{1 - \frac{\eta^2}{\eta_{t}^2}(1 - (\vec{d} \cdot \vec{n})^2)}$
+ If the term in the square root is negative, we get total internal reflection.
+ If this happens when the ray is leaving the object, there is no refracted
  light contribution.

### Intensity of Reflection and Refraction

+ Amount of reflection is function of angle of entry of incident ray.
+ Given by **Fresnel Equations**.
+ Approximated by **Schlick Approximation**.
  - Multiply by specular coefficient to get total specular contribution.
  - Multiply $1 - \text{Schlick Approximation}$ by refracted light to get total
    refracted contribution.

### Attenuation Through Refraction

+ Beer's Law.
+ Recursive: $I(s) = I(0)e^{-\ln(a)s}$
+ $s$ is distance the ray has traveled.
+ $a$ is unknown, found through trial and error.

## Distributed RayTracing

Two main problems w/ classical raytracing:
1. Images are too clean, sharp, and always in focus.
2. Lot's of aliasing around edges of objects and shadows.

These problems are caused by point sampling:
+ 1 ray per pixel
+ 1 ray to light source
+ 1 reflected ray
+ 1 refracted ray

Consider a row of pixels and their rays:
Rays are shot through a point on the pixel. but we ideally want to sample the
_whole_ pixel, not just a point on it. a ray may hit an object while the next
misses it.
+ a ray for one pixel may hit an object, but the ray for the next pixel might
  not (imagine the edge of the object is diagonal.)

The basic solution:
+ Use more than one ray per pixel
+ Easiest approach is to divide the pixel into smaller squares and use one
  array for each cell. This only delays (minimizes) the problem. Not a sol'n
+ Next approach is to randomly select a position within a pixel. Use a large
  number of rays per pixel. Average the result.
  - Not uniform without lots of rays.
  - Inefficient, needs large number of rays to get a good sample for each
    pixel.
+ Next approach: Combine both. Divide the pixel into a grid of smaller cells.
  - Within each cell, randomly sample.
  - pixels are sampled more uniformly.
  - but still somewhat random so aliasing isn't as noticable.
  - Adds a bit of noise in exchange for eliminating aliasing.
  - 5x5 grid is usually good enough.

### Shadows

Dark == umbra, transition/light == penumbra.

+ Light source must have finite area, cannot be a single point.
+ Surface of light source is divided into a grid similar to the pixel stuff
  above.
+ Instead of one shadow ray, use multiple, one for each of the cells on the
  light source.
+ The rays intersect a random point in each cell, also like above.
+ The values of the different shadow rays are averaged.

+ If a point is in the Umbra: all rays are blocked, same result as without
  distributed rays.
+ If a point is in the penumbra: only some rays blocked, average result with
  give desired affect.

+ Distributed rays also works for:
  - depth of field
  - glossy reflections
  - motion blur

### Efficiency

+ The number of rays increases rapidly.
+ Using 25 rays (for example) for a pixel, with _one_ light source w/ 25 cells,
  number of rays increase by factor of 625 at least.

## Path Tracing

The branching of subsequent rays in distributed raytracing leads to rays
contributing only a fraction of the colour of the pixel. We could get away with
fewer rays.

With PathTracing:
+ each of the initial multiple rays follows _only one_ path through the scene.
+ a number of rays for each pixel is still used (usually in the range of
  several hundreds to thousands.)
+ when a ray hits an object, randomly decide to generate a
  shadow/reflection/refraction ray.
  - Also, randomly generate a direction for the ray using the techniques as
    distributed ray tracing.
  - Continues until the ray leaves the scene or goes through enough levels.

### Advantages/Disadvantages

+ No explosive growth of rays
+ Deep in the tree, a large number of rays is wasteful since each contributes
  little to the final colour of a pixel.
  - Path tracing only uses one ray throughout.
+ Will need lots of rays to get a good average value.
+ Basically a Monte Carlo simulation of light transfer.
  - needs a large number of samples
  - We know that there are ways for controlling the sampling, so we can do
    this reasonably efficiently.

Photon Mapping:
+ both forward and backwards path tracing.
+ forwards from light "deposits" photons on surface of objects.
+ backwards from eye picks up these photons.
