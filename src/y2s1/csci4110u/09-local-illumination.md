# Local Illumination

Based on the information available at the current pixel or point on the object.

+ Fast compared to global illumination.
+ Uses only info available at the current pixel.
  - Material properties.
  - Light sources.
+ 3 components:
  - Ambient.
  - Diffuse.
  - Specular.

## Phong Model

+ **Ambient**: background illumination. Estimated, not considered in detail.
  - The environment has an ambient level $I_a$
  - Each object has ambient coefficient $k_a$.
  - The ambient reflection is: $I_a k_a$.
  - Has the colour of the object.
+ **Diffuse**: Reflected light.
  - Same in all directions.
  - Depends on amount of light coming in.
    * Depends on angle between the light dir and the normal of the surface at
      the point.
  - $I_d = I_i k_d cos\theta$
  - Or $I_d = I_i k_d (N \cdot L)$
  - Vectors should be normalized.
  - Has the colour of the object.
+ Specular reflection:
  - Shiny highlight.
  - Mirror like reflection.
  - Intensity depends on angle between reflection vector or the half vector.
  - $I_s = I_i k_s cos^n \theta$
  - Or $I_s = I_i k_s (R \cdot V)^n$
  - $R = -L + 2(L \cdot N)N$
  - $n$ determines shinyness. Large values is more shiny.
  - Computing R can be costly.
  - Could use the half vector instead:
    * $H = \frac{(V + L)}{2}$
  - Has the colour of the light source.
+ For multiple light sources, the contributions are summed.
+ May get overflow in colour computations.
+ Additionally, we may attenuate contribution from light sources with distance.
  - The correct attenuation is $\frac{1}{d^2}$.
  - Often we use a more general: $f(d) = \frac{1}{c_1 + c_2 d + c_3 d}$
  - $f(d)$ is computed for each light source and modifies that light's contribution.

## Textures

+ Need mapping between pixels on a surface and the pixels in the texture.
  - Texture coords (u, v).
  - Range [0, 1].
+ How do we assign texture coords?
  - For parametric surfaces: Use the (u, v) parameters of the surface.
  - For polygonal meshes:
    * Assign texture coords by hand.
    * Generate texture coords alongside procedural modeling techniques.
    * Compute some other way...
  - For relatively flat objects:
    * **Planar mapping**: Construct plane parallel to the object and
      construct a (u, v) coord system on the plane. Project vertices onto the
      plane via a parallel projection to get the (u, v) coords.
  - A **Cylindrical Mapping** can be used for cylindrical objects.
    * Use the bounding box of the cylinder to compute the center for use as the
      basis of the cylindrical coords.
  - **Spherical Map**:
    * Distorts the texture around the poles!

### Filtering

+ No issues with 1-to-1 mapping between texture coords and screen pixels. Otherwise...
+ **Magnification** 1 screen pixel = fraction of a texel.
+ **Minification** 1 screen pixel = many texels.
+ The texture could also be rotated with respect to the screen.
+ For minification:
  - Prefilter the texture.
    * This is an approximation.
    * Mipmap.
    * Mipmaps take up about 2x the memory (not too bad).

+ Environment maps.
  - Are cube maps
  - 6 textures, 1 for each of the +/- axis.
  - 3D coord texture.
    * Largest magnitude coordinate picks the texture to use.
    * Other two are regular 2D texture coords.
  - Usefull for reflections.
+ Texture coords can be computed with the normal at the vertex and the vector
  from the eye to the vertex/pixel (when in world coords.)
+ Create the cube map like so:
  - Position the camera at the object center.
  - point the camera along one of the 6 axis.
  - Render the scene without the reflecting object.
  - Save the result as one of the cube map textures.
  - Repeat for the other axises.

## Displacement Maps

+ Done at the vertex level.
+ Texture value is multiplied by the normal vector.
+ Result is added to the vertex position.
  - As a consequence, also changes the normal.
+ Requires a fine mesh otherwise the surface becomes too rough and distorted.
+ ...But, actually changes the geometry.

## Bump Maps

+ Done at the pixel level.
+ Doesn't change the geometry.
+ Changes the normal vector.
+ Done in **tangent space**:
  - $t = \frac{\partial s(u, v)}{\partial u}$ $\larr$ The "tangent".
  - $b = \frac{\partial s(u, v)}{\partial v}$ $\larr$ The "bitangent".
  - $u = \frac{t}{||t||}$
  - $v = \frac{b}{||b||}$
  - $n = u \times v$
+ The new surface point is: $s'(u, v) = s(u, v) + n(u, v) \cdot \text{bump map}(u, v)$
+ The new normal is $u' \times v' = \frac{\partial s'(u, v)}{\partial u} \times \frac{\partial s'(u, v)}{\partial v}$
+ Simplifications can be made if we assume the normal vector moves slowly from
  pixel to pixel.

## Normal Map

+ Store normals in textures.
+ Computed from higher res models.
+ Normals don't transform the same way as points:
  - Need to transform the texture to eye coords.
  - Alternatively, use tangent space.

## Procedural Textures

+ Can generate any resolution we need.
+ Easier to construct complex textures.
+ Can generate 3D textures easily.
+ Solid 3D textures provide values for the internal structure of an object.
  - Consider chopping down a tree.
+ 3D textures also provide values for partially transparent volumes, like smoke.
+ Many procedural textures bulit on top of Perlin noise.

## Perlin Noise

+ Random process defined on $\R^3$.
+ Rotation invariant.
+ Translation invariant.
+ Time invariant.
+ Band limited - Removed low and high frequencies from the random process.
+ Generate random values stored at lattice points.
  - Random _gradient_ of the process.
  - Assumed to be zero at lattice points.
+ Use interpolation to compute a value at any point in $\R^3$
+ Basis for many procedural textures.

### Construction

+ 1D array, $G$ of random gradients.
+ Map lattice points into this array.
+ The gradients must lie on the surface of the unit sphere.
  - Generate random 3D vectors, normalize them, and store in $G$.
+ Use a hash function to map from lattice points to the gradient table.
  - Based on a permutation table, $P$ of integers.
  - Given i, j, k lattice coords, compute index of $G$ using the permutation table.
+ Grab the random gradient by performing: $G$[hash(i, j, k)].
+ Multiply the retrieved gradient by a blending function for each corner.
  - Dot product against the fractional part of the inputs.
+ Sum the corner values to get the final value.

## Turbulence

+ Combines the noise function at different frequencies and scales.
+ $noise(2\vec{p})$ has twice the frequency as $noise(\vec{p})$.
+ Sums octaves.
  - Each octave is twice the frequency of the previous.
  - Half the amplitude.
+ Can be used to produce a wood/marble texture.

## Shadows

+ **Occluder**: Object that blocks light.
+ Multiple light sources.
  - Each casts different shadows.
+ Shadow has 2 parts:
  - **Umbra**: The dark center part, all light blocked.
  - **Penumbra**: Fuzzier edges of shadow where only some light is blocked.
+ **Hard Shadow**: Shadow with only an Umbra.

## Shadow Volumes

+ Construct a volume.
+ Near cap is the polygon.
+ Sides are constructed by a ray from the light source through the polygon
  vertices.
  - Defines polygon edge from near cap to the far cap.
+ Far cap is constructed by projecting the near cap to the end of the volume.
+ Any object in the shadow volume is in the shadow.
+ To determine if a pixel is in shadow:
  - Initialize a counter to 0.
  - Ray cast from eye to the pixel.
  - For every front facing shadow-volume face: Increment the counter.
  - For every back facing shadow-volume face: Decrement the counter.
  - If the counter is non-zero, the pixel is shadowed.
+ This is slow.

## Stencil Buffer

+ Buffer same size as the frame buffer.
+ Typically only 8 bits deep.
+ The stencil buffer algorithm:
  - Clear the depth and frame buffers.
  - Draw the model with only ambient light.
    * Update the z-buffer.
    * No need to worry about shadows yet.
  - For each occluder:
    * Construct a shadow volume.
    * Draw front facing polygons of the shadow volume.
      + Don't update the depth or frame buffer, but increment the stencil buffer
        for each pixel that passes the z buffer test.
    * Draw the back facing polygons of the shadow volume.
      + Decrement the stencil buffer for each pixel that passes the z buffer test.
  - Non-zero stencil buffer pixels are in shadow.
  - Draw with diffuse and specular lighting.
    * For each pixel, check the stencil buffer.
    * If zero, add the full colour to the frame buffer.
    * Otherwise leave the ambient light in the frame buffer.
+ Issue 1: With a large number of polygons in the occluder, we will generate lots of
  shadow volumes.
  - Could overflow the stencil buffer.
  - Drawing lots of shadow polygons could take time.
+ Solution: Construct the silhouette for the occluder from the light source
  position.
+ Use these edges to construct the shadow volume.
+ Only needs to be updated when the occluder or light source moves.
+ Issue 2: If the eye is inside a shadow volume or intersects the near clipping plane,
  the stencil buffer computation will be wrong.
+ Solution: Count the shadow volumes behind the depth buffer (pixels that fail the test).
  - If the count of front & back facing polygons is the same, the pixel _isn't_ in shadow.

## Shadow Maps

+ Observation: Shadows are hidden surfaces from the light source's point of view.
  - If an object is hidden, it is in shadow.
+ For a spot light (directional point light) we can use our standard rendering algorithms.
+ Render the complete model.
+ Save the _depth buffer_.
  - This is the shadow map.
  - Contains the distance from the light source (z-val) where shadows start for
    each pixel.
+ Render the model and:
  - For each pixel, transform to light space.
  - Test the pixel z against the shadow map.
  - If it is greater, the pixel is in shadow, use only ambient.
  - If it is lesser, the pixel is in light, use the full light model.
+ For directional light use a parallel projection instead of perspective.
+ For non-directional lights, use a cube map for a shadow map.
  - Render fram the light source _6_ times.
  - The shadow map is static unless the light source or any of the geometry
    changes.
+ Does handle shadows from transparent surfaces.
+ Can store partial coverage for smoother shadows. (Shadow map is at least 8 bits deep).
+ Shadows can alias.
  - Shadow computed at a fixed resolution.

