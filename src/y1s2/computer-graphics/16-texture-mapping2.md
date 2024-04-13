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
  * This solves the large size problem.
  * The fn can even filter the texture based as it goes.

## Bump Maps (Normal Maps)

+ A texture containing the diffuse light only. (<- how would we do this?)
+ Just generate a texture! (Programmatically, or other)
+ Bump Maps allow you to change the illumination handling in shaders with the
  use of a texture.
+ They are vector offsets for the normal vectors.

# Slide 65


