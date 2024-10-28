# Procedural Modeling

+ Techniques for producing large amounts of geometry with little user work.
+ **Database Amplification** - Start with a small database of primitives
  and generate many more.
+ Primitives produced can be stored for later display.
+ Could tune number of primitives to object size on screen.

## Subdivision Surfaces

+ Start with a polygonal mesh and apply a subdivision scheme to produce a
  refined mesh.
+ For triangles: Replace the original triangle with 4 sub triangles.
+ Can be applied to the whole mesh or just subsets.
+ Interpolating Subdivision
  - Original points on the surface are preserved.
  - Produce a finer grid that matches the original data.
+ Approximating Subdivision
  - The original points can move as the surface is refined.
+ We are mainly interested in approximating subdivision schemes.

## Subdivision Schemes

+ **Topology**: The insertion of new points into the mesh.
+ ** Geometry**: Computing the position of the new and old points.

### Catmull-Clark (Quad Subdivision) Topology

+ Each quad is divided into four sub quads.
+ 5 new points:
  - 1 in the center of the quad.
  - 4 at the midpoint of each edge.
+ Results in a new (finer) quad mesh.
+ Each internal vertex has four edges leading in/out of it.
  - Called the **Valence** of the vertex.
  - A valence of 4 means the vertex is **Ordinary**.
  - Any other valence means the vertex is **Extraordinary**.
  - Extraordinary verticies will always remain in the mesh.
  - Extraordinary verticies cause continuity problems (Usually, it drops by one).

### Loop Subdivision Topology

+ Each triangle is subdivided into four.
+ Vertices are inserted at the midpoints of each edge.
+ Ordinary vertices have a valance of 6.

## Catmull-Clark (Again) Geometry

+ The center point is called a **Face Point**.
+ The position of the face point is the average of the original four points.
+ The position of the four new edge points is:
  - The average of:
    * The average of the two original vertices at the ends of the current edge.
    * The average of the two face points on either side of the edge.
+ The new position of the original four points is:
  - $\frac{F + 2R + (n - 3) P}{n}$
  - $P$ is the vertex.
  - $n$ is its valence.
  - $F$ is the average of the $n$ face points for the faces that touch $P$.
  - $R$ is the average of th $n$ edge midpoints that are incident on $P$.
+ Can construct a matrix that transforms the original vertices into the
  vertices for the 4 new quads.
+ $C^2$ continuous everywhere except at extraordinary vertices.
  - In this case, it is $C^1$.
+ Basically constructs a B-Spline surface.
+ Edge based data structures are a natural choice for their connectivity info.

## Fractals

+ Broad class of techniques.
+ Based on the notion of self similarity.
+ Patterns repeat if an object is scaled up or down.
+ Exact self similarity only appears in mathematical objects.
  - We want more interesting objects so we include some randomness.
+ **Fractal** - An oboject is statistically self similar if its statistical
  properties are stationary as the object scales.
+ **fractional Brownian Motion** - A process that scales by $h^{-H}$ as the
  object is scaled by $h$.
  - $H$ is the fractal dimension which varies from 0 to 1.

## A Simple Fractal Process

+ Start with a line.
+ Compute its midpoint.
+ Add a random displacement.
+ Apply the process recursivel to the two line segments.
+ What random process do we use?
  - A gaussian process scaled by the subdivision of the curve.
  - It must be fractal, i.e. statistically self similar.
    * Scale standard deviation by $2^{-H}$ on each subdivision.
  - Must return the _same_ random number for a given point in space.
    * So when scaling in and out, points don't move around.
+ The random number generator should be a function of the point that is being displaced.
  - Can create a large table of random numbers (table size is usually a prime number.)
  - Hash, e.g. the point's coordinates, to get the index into the table.
  - Or hash the (u, v) coordinates from parameter space.
    * Position coordinates of a point could change after transformations.
+ Could displace in a particular direction (y-dir for horizontal-ish line).
+ More generally, displace in the normal direction.

## Fractal for Surfaces

+ For triangles:
  - Displace the midpoints of its edges.
  - Construct 4 new triangles.
+ For quads:
  - Displace the midpoints of its four edges.
  - Connect opposing new midpoints with a line.
    * Displace the midpoint of these new lines.
  - Average the displaced midpoints of the two new lines to get the center point.
+ Edge based data structures are good for this.
  - Each edge will only be subdivided once.
+ Indexed face sets or polygon soup may subdivide an edge twice.
  - We would need the same displacement both times or the mesh breaks apart.
  - Hashing for the random number will result in the same number both times.
  - What about displacement direction?
    * Displacing in the normal direction gives two different directions. Each
      connected side has a different normal.
    * Solution: Store normals with the vertices and average them.
+ Algorithms are stated recursively, usually.
  - Too expensive.
+ Iterative algorithms are used instead.
  - All subdivisions at one fractal level are done first.
  - Then move on to the next level.

## Terrain

+ Usually seen as flat in parameter space.
+ 2D grid with a height value at each grid point.
+ Only need to store the height value.
  - Called a **Height Field**.
+ For small terrains, a flat grid with height from a height field is good.
+ For large terrains, polar coordinates with grid points at constant lattitude
  and longitude increments works well.
  - Assume a constant radius.
  - Compute point on sphere.
  - Height field displaces in normal direction.

### The Diamond-Square Algorithm

+ Two passes over the height field.
  1. Compute the center of each quad plus the random displacement.
  1. Compute the edge points plus the random displacement.

## L-Systems

+ Used to model botanical objects.
+ Standard technique for modeling plants.
+ Consists of:
  - A grammar.
  - Rewrite rules, An interpretation of the strings in the grammar.
+ **Initiator** - The original string.
+ **Productions** - Rewrite rules applied in _parallel_.
+ Turtle graphics is an example.
+ Run infinitely.
  - Should be stopped after a fixed number of generations.

## Particle Systems

+ A modeling and animation technique.
+ Based on simple particles with no real shape, a pixel in size.
+ Simple ballistic motion.
+ Can draw the particle or its path.
+ Start with position $p$ and velocity $v$.
+ At each time step, compute:
  - $p = p + \Delta t * v$
  - $v = v + \Delta t * a$
  - $\Delta t$ is the time step.
  - $a$ is acceleration.
+ Particles can be given a random lifetime or can be terminated when they reach
  a specific level (e.g. ground level).
+ Particles typically have a number of randomly generated properties.
+ Partciles are generated from a source.
  - Usually a simple shape.
  - Initial positions are randomly distributed over the source.
+ The general algorithm:
  - Move existing particles.
  - Remove dead particles.
  - Generate new particles.
  - Draw particles.
+ Don't generate all particles at the same time point.
  - They will end up bunched together when drawing.
  - Use a random birth time.
+ How particles are drawn differs:
  - Fire: Each particle adds colour to the pixel it covers.
    * Reduce particle colour over time.
  - Grass: Draw the particle path once the system is complete.
  - Fireworks: Draw the path as the particle moves.
+ Lots of parallelism $\rarr$ Ideal for GPU implementation.
