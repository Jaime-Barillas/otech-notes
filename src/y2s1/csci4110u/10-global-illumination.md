# Global Illumination

Based on light transfer in the entire environment.

+ Local illumination can't do transparency well.
+ Inter-reflections.
+ Soft Shadows.
+ Lighting effects (like area lights).
+ These effects require global knowledge.
+ **Global Rendering Algorithms** - algos that consider the entire environment.
+ Raytracing.
+ Radiosity.

## Classical Ray Tracing

+ Trace rays of light from light sources to the eye.
  - Forward raytracing.
+ Easier to trace from eye to light sources.
  - Backwards raytracing.
+ Follow the ray until it strikes an object.
  - Compute colour of the object where the ray hit.
  - Requires computing light contribution of other objects in the scene.
  - Trace ray from intersection point to a light source.
    * A clear path $\rarr$ compute diffuse & specular light.
    * A blocked path $\rarr$ the object is in shadow, use only ambient light.
    * Only works for point light sources, and ones that are infinitely far away.
+ If the ray strikes another object, we need to compute the colour of that
  object as well.
  - This is the reflected colour/highlight (specular).
+ Recursive nature.
+ If the object is transparent:
  - Trace through the object: **Refraction**.
+ Stop tracing based on a heuristic:
  - Recursive/depth level reached.
    * In particular, the height of the ray tree.
  - Light contribution too small.
+ **Primary Ray**: The original ray.

### Ray Tree

+ Collection of primary/reflected/refracted/light rays.
+ Pixel is the root.
+ Each intersection is a node.
+ The tree can grow quite large $\rarr$ ray tracing is expensive to compute.

## Classical Ray Tracing

+ Want to control the number of rays.
+ Intersection computation should be as efficient as possible.
+ Want only the closest intersection, others are hidden behind it.

## Details

+ Ray calculations can be done in either:
  - Eye coord system.
  - Model coord system.
+ Either is fine since this is only a small part of the overall computation.

### Intersection

+ Most expensive operation.
+ $p(t) = e + td, t>0$
  - $e$ is the start point.
  - $d$ is the direction of the ray.

Sphere intersection
+ One of the easiest.
+ Vector form of the equation for a sphere: $(\vec{p} - c)(\vec{p} - c) - R^2 = 0$.
+ Substitute ray equation for $\vec{p}$: $(e+td - c)(e+td - c) - R^2 = 0$.
+ This simplifies to a quadratic equation.
+ 2 solutions:
  - If both roots are complex, the ray does not hit the sphere.
  - Want the smallest _positive_ root.

General Polygon Intersection
+ First, determine where the ray intersects the polygon's _plane_.
+ Then, determine whether the intersection point is inside or outside the polygon.
+ For a polygon vertex $p_1$ and normal to the _polygon_ $n$, the plane equation is:
  - $(p - p_1)n = 0$.
+ Again, substituting the ray equation: $(e + td - p_1)n = 0$
  - In terms of $t$: $t = \frac{n(p_1 - e)}{nd}$
  - If $nd = 0$, the ray is parallel to the plane and there is no intersection.
+ Now we have our intersection point somewhere on the polygon's _plane_.
+ How do determine if that point is _inside_ the polygon?
  - Project the polygon and intersection point onto one of the coordinate planes.
  - Draw a line from the intersection point to infinity (direction doesn't matter!)
  - Count the number of times the line intersects with a polygon edge.
    * Odd $\rarr$ Point is inside polygon.
    * Even $\rarr$ Point is outside polygon.
  - NOTE: Only works for convex polygons.

## Bounding Volumes

+ Want to detect _non_-intersections as quickly as possible.
  - Especially for objects that require numerical techniques.
  - Or objects with lots of polygons.
+ Solution: Enclose them in a simpler shape.
  - e.g. A sphere or Box.

## Grid Intersection

+ The ray only traverses a small part of the scene.
  - Won't traverse a large portion, don't need to check for intersections here.
  - Test objects most likely first.
+ Build grid on top of scene.
+ Assign objects to grid cells they occupy.
+ Each cell should hold a small number of objects (3 to 5).
+ Start with the cell the ray originates from.
  - Test objects assigned to that cell.
  - If an intersection is found, we are done (take the closest.)
  - Otherwise, continue onto the next cell the ray would travel through.
+ With an n\*n\*n grid, worst case: Examines $3^\frac{1}{2}$ cells.
+ If objects are _evenly_ distributed, save time on the order of $\frac{1}{n^2}$.
  - It's rare that objects are evenly distributed in practise.
+ Datastructure grows in space on the order of $n^3$.
+ If cell sizes get too small, an object may be assigned to multiple cells.
  - May do the intersection computation multiple times.
+ Computing the next cell to visit takes time as well.
+ Thus, there is a lower limit on cell size.
+ Solution: Adaptive grid size.

## Reflections

+ Compute direction of the reflected ray from the original ray direction and
  the surface normal: $r = d + 2(d\cdot n)n$.
+ Apply the ray tracing computation recursively to get the colour.
  - Find the intersection of this reflected ray at $t > 0$ to ensure we don't
    use the previous intersection point.
+ Reflections contribute to specular.
  - Track the product of the specular coefficients through each recursive level.
  - Once it goes below a threshold, stop tracing the reflected rays.

## Refractions

+ Objects can both reflect and refract light.
+ Use **Snell's Law** to determine angle of refraction.
+ Happens twice, once on entering an object, once on leaving an object.
+ Gives us an angle, but not a direction.
+ After some math, results in an equation with a squar root.
  - If the term in the square root is negative, we have total internal reflection.
    * If it happens on entering the object, light is purely reflected.
    * If it happens on _exiting_ the object, there is _no_ refracted light
      contribution.

### Reflection/Refraction Intensity

+ A function of the incident angle.
+ Given by the **Fresnel equations**.
+ Use the **Schlick approximation**.
  - Google the math yourself.

## Refractions

+ Light will attenuate.
+ **Beer's Law**: $d I = -C I dx$ or $\frac{dI}{dx} = -C I$
+ Ultimately after some math and initial conditions:
  $I(s) = I(0)e^{-ln(a)s}$.
  - $s$ is the distance that the ray traveled through the material.
  - $a$ is the attenuation constant, determined experimentally.

## Radiometry

+ Prerequisite to Radiosity.
+ **Radiant Power** or **Flux**: Amount of energy that flows through a surface
  per unit time.
  - Denoted $\phi$.
  - Measured in Watts.
+ **Irradiance**, $E$, is the amount of _incident_ flux _per unit of surface area_.
  - Measured in $\frac{Watts}{m^2}$
  - Expressed in the following way: $E = \frac{d\phi}{dA}$
+ **Radiosity** or **Radiant Existence** is the _exitant_ flux per surface area.
  - Expressed in the following way: $M = B = \frac{d\phi}{dA}$

![Radiosity Incident/Exitant](10-radiosity.png)

+ Flux, Irradiance, and Radiosity are wavelength dependant.
  - Assume there are at least R, G, and B versions of them.

## Radiosity

To begin with, assuming we have only light sources and diffuse reflectors
(And lights are area lights.):

+ Consider radiosity B(x) for a point on a surface.
+ Express it as:
$$
B(x) = B_e(x) + \rho(x) \int_s K(x, y) B(y) dA_y
$$
+ $B_e(x)$ is the light emitted at the point(x), $s$ is all the surfaces in the
  model, and $K(x, y)$ is a geometry term given by:
$$
K(x, y) = G(x, y)V(x, y)
$$
+ $V(x, y)$ is $1$ if point x is visible from point $y$, otherwise it is $0$.
+ $G(x, y)$ is a geometry term given by:
$$
G(x, y) = \frac{\cos\theta_x \cos\theta_y}{\pi r^2}
$$
+ $\theta_x$ is the angle between the normal at x and the line joining $x$ and $y$.
+ $\theta_y$ is defined the same.
+ $r$ is the distance between $x$ and $y$.

+ Cool, but what does it mean?
+ Radiosity at x is divided into 2 parts:
  1. The amount of radiant energy emitted at point $x$.
    - If $x$ is a light source.
  1. The amount of light from all other points in the scene that is then
    reflected at x.
+ The equations are all in terms of differential properties.
+ Hard to implement.
+ Want equations that deal with finite patches.
+ When dealing with patches, assuming radiosity is constant across a patch, and
  the area of patch $i$ is $A_i$...
+ We can restate the formula in terms of _irradiance_.
+ Through some math, after dividing through by $A_i$ and replacing the integral
  with the **Form Factor**, $F_{i,k}$ (a constant between two patches):
$$
B_i = E_i + \rho_i \sum^n_{k=1} B_k F_{i,k}
$$
Or in matrix form (Matrix $F$ contains the form factors and reflection coefficients):
$$
(I - E)B = E \\
B = (I - F)^{-1} E
$$

+ Two issues:
  1. $n$ is very large so the matrix will be quite large.
  1. Where do we get our form factors?
+ For the first issue:
  - Simplify by $K = (I - F)$.
  - The equation is now: $KB = E$.
  - K is an $n$*$n$ matrix and bothe $B$ and $E$ are $n$*1 vectors.
  - K may be quite large but is "well behaved", so iterative techniques to
    invert it exist and are sufficently fast.
+ For the second issue:
  - The form factors represent a two way flow of energy.
  - $F_{i,k}$ can be interpreted as:
    * The fraction of irradiance on $i$ that originates from $k$.
    * The fraction of the power emitted by $i$ that reaches $k$.
  - Form factors have the following properties:
    * They are $\ge 0$.
    * In a closed environment, they sum to $1$.
    * $A_i F_{i,k} = A_k F_{k,i}$
  - The last property lets us write the radiosity equations multiple ways.
  - How do we compute the form factors?
    * Sometimes analytically.
    * Sometimes we can break down a shape into simpler pieces with analytical solutions.
    * Most of the time, we use an analytical technique.

## The Hemicube Algorithm

+ The standard way to compute form factors.
+ Construct 5 images centered on patch $i$.
  (5 sides is almost a complete cube, hence "Hemicube".)
+ Run a standard z-buffer algorithm with the camera at the center of patch $i$
  for each of the 5 surfaces.
+ Instead of storing the colour in the frame buffer, we store the index of the
  closest patch $k$.
  - This gives the value of $V(i, k)$ for each direction above the patch.

![Hemicube Algo](10-hemicube.png)

+ Computing $F_{i,k}$ is summing the contributions from the pixels that have
  the value $k$
+ Multiply by a modified pixel area in the expression: $\frac{\cos\theta_i\cos\theta_k}{\pi r^2}$.
  - This needs to be done since the projected area of patch $k$ is distorted by the hemicube.
