# Texture Mapping Part 2

+ OBJ model files often contain texture coordinates for each vector. They are
  typically stored after vertices and normals.
+ Aliasing issues can occur. Can be solved by tiling the texture(multiply
  tex-coords by e.g. 8) or generating mipmaps (glGenerateMipmap.)

## OpenGL and Image Data

+ OpenGL expects a packed representation of the image: One pixel after the other
  in memory, line-by-line.
+ FreeImage allocates one line at a time _at word boundaries_.
  * The data FreeImage loads will need to be packed so OpenGL can work with it.
  * Process the image data one line at a time, moving the pixels from the
    FreeImage internal storage to an array we'll pass to OpenGL.

## Computing With Textures

+ The values retrieved from a texture can be used in any computation, including
  in vertex shaders.
+ The values don't need to be colour info (e.g. normals, bump maps, etc.)
+ More than one texture map can be used at a time.

## Multiple Textures

+ Multiple texture coordinates needed.
+ Vertex and fragment shaders will need an additional attribute for the
  textures.
+ Each texture can be associated with a **Texture Unit**. There are at least
  80 available (0 through 79).
+ `glActiveTexture` selects one of these texture units.
+ Next, use `glBindTexture` to associate a texture with the active texture unit.
+ To then associate a texture with a shader sampler variable, we need to assign
  the texture unit number to that sampler with `glUniform1i`.

## Advanced Texture Mapping

+ We've done simple 2D texture mapping.
+ We can use 1D and 3D textures.
  * 3D examples: Clouds, smoke, tree trunk (cut it open to see the inside
    texture.)
+ OpenGL handles them simillarly to 2D images, albeit with different functions.
+ 1D textures are essentially a line. Often used for contour lines and scales on
  the surfaces of objects.
+ 3D textures are a volume of pixel values. We are essentially sampling a
  volume instead of a surface.
  * 3D textures are large, lots of memory needed!
  * Hard to filter to get rid of aliasing problems.

## Procedural Textures

+ 3D textures can be very large.
+ We can generate the values in a function instead.
  * This solves the large file size problem.
  * The fn can even filter the texture based as it goes.

## Bump Maps (Normal Maps)

+ A texture containing the diffuse light only. (<- how would we do this?)
+ Just generate a texture! (Programmatically, or other)
+ Bump Maps allow you to change the illumination handling in shaders with the
  use of a texture.
+ They are vector offsets for the normal vectors.

## Environment Maps

+ 6 textures, one for each face of a cube.
+ 3D texture coords.
  - Largest one determines which image (cube face) to use.
  - Other two are regular texture coords.
+ Cube maps have the advantage over sphere maps that there is less distortion
  around poles.
+ Used to capture reflection of other objects in scene.
+ To compute tex coords:
  - In a cube-map: normal at vector or vector from center of object to surface
    position (pixel to draw).
+ For reflection uses specifically:
  - Need $\vec{u}$, vector from origin to the vertex/pixel.
  - Need $\vec{n}$, normal at vertex/pixel.
  - In eye coord-system.
  - If in world coord-system: $\vec{u}$ is vector from eye to vertex/pixel.
  - Reflection direction is: $\vec{r} = \vec{u} - 2\vec{n}(\vec{n} \cdot \vec{u})$
  - $\vec{r}$ is used as the texture coordinates.
+ To create the texture map:
  - Position camera at object center.
  - Point in direction of each plane (+/-x, +/-y, +/-z)
  - Render scene without the object.
  - Save each render as an image.
  - Can be low resolution
  - Cheap way to get global illumination effects.
