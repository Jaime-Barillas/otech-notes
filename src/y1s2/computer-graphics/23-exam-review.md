# Exam Review

* Short answer questions - similar to mid-term.
* Be precise in your answers.

## Graphics Pipeline

+ LCDs, rectangular array of pixels, resolution
+ Colour, RGB, gamut, scanning pattern, graphics memory - frame buffer
+ Rendering, global vs. local illumination
+ Pipeline: modeling, projections, viewing, lighting and colour – vertex
  processing
+ Hidden surface, pixels covered by triangles, lighting – fragment processing

### Possible Questions

+ How is colour represented in computer graphics?
  > + 3 components per pixel.
  > + R - Amount of red light.
  > + G - Amount of green light.
  > + B - Amount of blue light.
+ What is a frame buffer?
  > + A 2D array of memory within the GPU.
  > + Usually, as large as the display resolution.
  > + Contains an entry for each pixel value.
  > + The GPU produces an electrical signal that transfers the pixel values
  >   from the framebuffer to the display.
+ What operations are performed in vertex processing?
  > + Each vertex is transformed into global world coordinates.
  > + Project verticies using desired projection.
  > + Clip triangles outside of viewing frustum.
  > + Run vertex shader
  > + Continue on to processing fragments.

## OpenGL Programming

+ GPUs, vertex and fragment shaders, GLSL
+ Buffer objects – vertices, normals, indices, loading data into buffers,
  vertex array objects
+ Building shader programs, attribute and uniform variables
+ GLFW and GLEW – window creation, display functions, keyboard functions
+ GLM – transformations, viewing, projections
+ Location of uniform variables, setting uniform variables
+ Transformations in vertex programs, simple light models in fragment shaders
+ Constructing projection and viewing matrices
+ Simple animations, transformations
+ Loading OBJ files, dynamic memory allocation
+ Handling multiple models, dealing with scale issues

### Possible Questions

+ What is a fragment shader?
  > + A GPU program.
  > + Runs for each fragment of a rendered object.
  > + Determines the colour of the corresponding pixel.
+ How does a uniform variable get its value?
  > + A uniform variable is declared in the shader.
  > + The CPU program construct/loads the uniform data.
  > + The CPU program determines the location of the uniform variable.
  > + The uniform data is transfered to the GPU by the CPU program.
+ What is the difference between an attribute and a uniform variable?
  > + Attribute variables change for each vertex.
  > + A uniform variable stays constant over a draw call applying to many
  >   vertices.
+ How are vertex coordinates passed to a vertex shader?
  > + Vertex coordinates are copied to an array buffer.
  > + An attribute variable for the vertices is added to the vertex shader.
  > + CPU programs determine the location of the attribute variable and link it
  >   to the data in the array buffer.

## Modeling

+ Coordinate systems, polygons, triangles
+ Meshes, vertex and face tables, computation of normal vectors, polygon and
  vertex normals
+ Transformations: translate, rotate and scale, transformation matrices,
  combining transformations, homogeneous coordinates
+ Scale and rotate about a point
+ 3D transformations, problems with rotation, rotation about an arbitrary axis,
  transforming normal vectors
+ Hierarchical modeling, parts and subparts, tree structure, objects at nodes,
  transformations on edges, car example
+ Masters and instances
+ Stickman example in OpenGL, cylinder procedure -> masters
+ Animation of hierarchical models
+ Implicit representations, $f(p) = 0$, points on surface satisfy equation
+ Parametric representations, position as a function of a parameter, two
  parameters for 3D surfaces
+ Piecewise representation, continuity, Cn and Gn continuity
+ Parametric curves, polynomials, canonical representation, blended
  representation, control points
+ Constraint matrix, blending matrix, general technique for fitting control
  points
+ Cubic curves, Hermite curve, knots, piecewise representation, continuity
  schemes
+ Local and global control
+ Bezier curves, convex hull, variation diminishing property
+ Generalization to 3D surfaces

### Possible Questions

+ What data structures are used to store a polygonal mesh?
  > Both a vertex table and face table.
  > The vertex table stores:
  > + Vertex position.
  > + Corresponding normal values.
  > + Additional information, such as, vertex colour.
  > The face table stores:
  > + indicies into the vertex table for each face.
  > These structures save space, allow you to process each vertex once, and map
  > cleanly to OpenGL array/element buffers.
+ What is the main difference between Cn continuity and Gn continuity?
  > With $C^n$ continuity, the derivatives of two curve segments must be equal.
  > With $G^n$ continuity the derivatives just need to be proportional to each
  > other (same direction, but different lengths.)
+ Given the matrix M for transforming vertices, how can the matrix for
  transforming normal vectors be constructed?
  > Take the transpose of the inverse of the original matrix. Rotations and
  > Translations can use the original matrix since they are considered
  > Rigid-Body transformations.
+ What are the three standard transformations?
  > + Scaling.
  > + Rotation.
  > + Translation.
+ What is an implicit representation of a curve or surface? What is the
  implicit representation of a circle?
  > Each point $p$ on a curve or surface satisfies $f(p) = 0$ for some function
  > f. The implicit representation of a circle is $(x - c_x)^2 + (y - c_y)^2 - r^2 = 0$
+ Why are masters and instances used in hierarchical modeling?
  > They are used when several objects have the same geometry. The master
  > stores the geometry, the instances point to the master. Advantages include:
  > Easy to change the geometry, change the master and all instances change
  > too. Saves memory since there is only one copy of the geometry.
+ Why is local control an important property of a curve?
  > With local control, changing a control point only affects a small part of
  > the curve. This makes it much easier to modify or edit a curve since making
  > a single change does not affect the entire curve.
+ Construct a transformation matrix that scales an object by (sx, sy, sz) about
  the point (x, y, z). You do not need to multiply out the matrices.
  > $$
    \begin{bmatrix}
      1 & 0 & 0 & x \\
      0 & 1 & 0 & y \\
      0 & 0 & 1 & z \\
      0 & 0 & 0 & 1
    \end{bmatrix}
    \begin{bmatrix}
      sx & 0  & 0  & 0 \\
      0  & sy & 0  & 0 \\
      0  & 0  & sz & 0 \\
      0  & 0  & 0  & 1
    \end{bmatrix}
    \begin{bmatrix}
      1 & 0 & 0 & -x \\
      0 & 1 & 0 & -y \\
      0 & 0 & 1 & -z \\
      0 & 0 & 0 &  1
    \end{bmatrix}
    $$

## Rendering

+ Viewing transformation, canonical viewing volume, transformation to screen
  space
+ Projection, parallel and perspective, orthographic projection matrix
+ Lookat transformation, transformation from model space to eye space
+ Perspective projection, projection matrix, use of homogeneous coordinates
+ Hidden surface removal, why we need to remove hidden surfaces, back face
  culling
+ Z-buffer algorithm, implemented in hardware, buffer resolution problem,
  non-linear z value, z fighting, why its worse with perspective
+ BSP trees – binary tree based on polygon plane, independent of eye position
+ Display and construction algorithms
+ Local and global illumination, illumination models
+ Phong Model: ambient, diffuse and specular reflection
+ Diffuse reflection, Lambert’s cosine law
+ Specular reflection, cosine to a power, half vector
+ Material colour, multiple light sources
+ Flat, Gouraud and Phong shading
+ Phong model in OpenGL, fragment shader
+ Directional light, easiest to implement
+ Point light, finite light position, eye coordinates or model coordinates
+ Spot light, cone of light at a finite position, light points in one direction
+ Computations in vertex shader produce Gouraud shading

### Rendering - Textures

+ Use images to add realism, texels, texture coordinates, size mismatch:
  texels/pixels, aliasing, sampling, averaging of texels, mipmaps
+ Texture mapping in OpenGL, texture function in fragment shader, reading
  textures from files, FreeImage, image structure
+ Multiple textures, texture units, 1D and 3D textures, procedural textures,
  bump mapping, environment maps

### Rendering - Ray Tracing

+ Ray tracing, trace ray from eye through pixel, intersect closest object,
  shadow rays, reflection rays, refraction rays
+ Ray tree, stopping conditions
+ Intersection, ray equation, implicit representation, substitution and solve,
  ray intersection with sphere
+ Polygon intersection, plane intersection, point in polygon
+ Bounding volumes, increase efficiency, bounding sphere, bounding box
+ Space partitioning, look in the most likely spot, grids
+ Algorithm, shadow rays, reflection ray computation, refraction computations,
  Snell’s law
+ Schlick approximation, Beer’s law
+ Distributed ray tracing, multiple rays per pixel, multiple shadow, reflection
  and refraction rays
+ Soft shadows, path tracing

### Possible Questions

+ What is the main difference between parallel and perspective projections?
  > Perspective projections produce realistic images while parallel projections
  > produce images where lengths and angles can be accurately measured.
+ What is z fighting and how can it be avoided?
  > Z fighting occurs when the z coordinates of two polygons fall into the same
  > bin in the z buffer; the z buffer doesn't have enough resolution to
  > distinguish them. Carefully set the near and far clipping planes to
  > increase the resolution of the z buffer bins.
+ What is the main difference between Gouraud and Phong shading?
  > Gouraud:
  > + Computes colours at the vertices.
  > + Linearly interpolates this colour over the polygon.
  > + Uses interpolated normal calculated from the normals of the vertices.
  > Phong:
  > + Interpolates the normals over the polygon.
  > + Computes colours at each pixel.
  > + Highlights are shown correctly.
  > + Computationally more expensive.
+ What are the three components of the Phong lighting model?
  > Ambient lighting - The "base" background light level.  
  > Diffuse reflection - Light scattered by an object equally in all
  > directions.  
  > Specular reflection - Highlights, concentrated around the mirror reflection
  > direction.
+ What are two techniques that can increase the efficiency of ray tracing?
  > Space partitioning.
  > + Bounding volume hierarchies.
  > + BSP.
  > + Octrees.
  > Parallelize ray tracing, use more cores.
+ What is the Schlick approximation used for?
  > + The Schlick approximation is used to approximate the intensity of the
  >   reflected and refracted light.
  > + It approximates the Fresnel equations.
  > + It is multiplied by the specular reflection coefficient to give the total
  >   specular reflection of the surface.
  > + $1 - \text{Schlick Approximation}$ is multiplied by the refracted light
  >   to give the refracted contribution.
+ What is the main difference between distributed ray tracing and path tracing?
  > Distributed Ray Tracing:
  > + Uses more than one ray per pixel.
  > + Uses mor than one ray per reflection/refraction.
  > + Each pixel is divided into a grid of cells. Within each cell, a random
  >   position for each ray is generated.
  > + Average the results. Simillar is done with light sources.
  > + Computationally expensive as number of rays grow.
  > Path Tracing:
  > + Uses more than one ray per pixel (a lot more.)
  > + Follows one path through the scene.
  > + When a ray hits an object, randomly decide to generate a shadow,
  >   reflection, or refraction ray.
  > + Randomly generate a direction for the ray as with Distributed Ray tracing.
  > + No explosive growth at each intersection.
  > + Easy to control the number of rays.
+ What is backface culling? What is it used for?
  > Backface culling detects the polygons that are pointing away from the
  > viewer: $\vec{n} \cdot \vec{v} > 0$. It is used to reduce the number of
  > polygons and pixels that must be processed.
+ What is the main difference between Phong specular reflection and Blinn-Phong
  specular reflection?
  > Phong uses the reflection vector while Blinn-Phong uses the half vector in
  > computing specular reflection.
+ What is the main difference between a point light source and a spot light
  source?
  > A point light source emits light in all directions equally. A spot light
  > source only emits light in a cone.

## Colour

+ What are two techniques that can increase the efficiency of ray tracing?
+ What is the Schlick approximation used for?
+ What is the main difference between distributed ray tracing and path tracing?

### Possible Questions

+ What is a metamer?
  > Two spectra that look (graphically) different, but produce the same colour.
+ Why can’t the RGB colour system be used to produce all visible colours?
  > It would require negative colour.
+ What is the main difference between the RGB colour space and the L*u*v*
  colour space?

## Visualization

+ General architecture: data, preprocessing, extract, display
+ Data: collection of data, file construction
+ Preprocessing: put data into usable format
+ Extraction: extract parts of data we are interested in
+ Display: display the data
+ Scale: large amounts of data, larger than workstation, in-situ visualization,
  computational steering
+ Case study: hurricane Isabel simulation
+ Preprocessing: convert separate U, V, W files into one wind velocity file
+ Extraction: compute particle paths, seeding
+ Display: line and tubes
+ Convert variables to 3D texture, display plane within texture, interactively
  control position and orientation of plane
+ Volume rendering, two components: voxels that contribute to a pixel, transfer
  function
+ Trace ray through volume, uniform sample, voxel by voxel
+ Transfer functions: maximum value, average value, distance to value and
  composite
+ Shaders for average value, opacity, extraction of structure, fragment shader
  for extracting low pressure area
+ Lighting the volume

### Possible Questions

+ What is in-situ visualization?
+ What is seeding and describe one of the techniques that can be used for
  seeding?
+ Describe two transfer functions.
