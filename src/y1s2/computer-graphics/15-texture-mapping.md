# Texture Mapping

## Bump Mapping
Use of texture to modify normals to simulate surface texture (bumpiness).

## Texture Mapping
Purpose: Map an image of a texture onto a _small_ number of polygons.
Images are 2D array of pixels. The surface of our object is 2D. Therefore map a
point on the surface to a point in the image.  
Cordinate system needed for object surface to map to image: **Texture
Coordinates**. Attach texture coords to the vertices.

Tiling textures is a thing. Texture coords go between [0, 1] normally. When
tiling they can go outside this range (e.g. [0, 2]). In this case we can look
only at the fractional parts of the coords (1._53_) and multiply against nx and
ny to get pixel coords in the image.

Works fine if image pixels and screen pixels are about same size. But...

### Screen Pixels << Texels
+ Several screen pixels have same texture pixel; result in same colour.
+ Results in a blocky effect.
+ Happens when you get too close to an object.
+ Possible solutions
  * Higher resolution texture.
  * Prevent user from getting too close to an object.

### Screen Pixels >> Texels
+ One screen pixels cover multiple texture pixels.
+ Visual artifacting when user moves.
+ Possible solutions:
  * Choose closest texel: Can cause **Aliasing**, specifically **bubbling** -
    When the user moves different texels will be chosen causing the artifacts.
  * Average the pixel colour of the multiple texels.

#### Averaging Texels
+ Will produce a blurry image.
  * Usually happens when objects are far away so blurryness is not as bad.
+ Relatively expensive.
  * Far away objects = more pixels to average.
  * Not a problem for non-realtime rendering.
+ Speed Ups:
  * blurr textures ahead of time (helps).
  * average a few of the close pixels.
  * Still problematic for large size mismatches.

## MipMapping
Solution to Averaging Texels at large distances.
+ Hierarchy of textures.
+ Typically pre-computed.
+ OpenGL has a procedure for this.
+ Memory cost is more, but less than twice.
+ Textures sizes must be powers of 2.
+ Base image is the original image.
+ Next level is quarter of the size.
  * Each pixel is average of 2x2.
+ Next level is quarter of the size again.
  * Repeat until dimensions of last texture is 1x1.
+ Select the texture at the level that best corresponds to the screen pixel
  size (texel size. approx equal pixel size.)
+ Alternatives interpolations exist:

### GL_NEAREST_MIPMAP_NEAREST
+ Select nearest mipmap level.
+ Select nearest pixel.

### GL_LINEAR_MIPMAP_NEAREST
+ Select nearest mipmap level.
+ Linearly interpolate pixel in 2X2 neighbourhood.

### GL_NEAREST_MIPMAP_LINEAR
+ Select 2 adjacent mipmap levels.
+ Select nearest pixel.
+ Linearly interpolate.

### GL_LINEAR_MIPMAP_NEAREST
+ Select 2 adjacent mipmap levels.
+ Linearly interpolate pixel in 2X2 neighbourhood for both mipmaps.
+ Linearly interpolate.

## Procedural Texture
+ Created within the program by a function.
+ Relevent OpenGL funcs:
  * `glGenTextures(_type_, _mipmap-level_, _gpu-pixel-format_, _width_, _height_, _border-size_, _cpu/file-pixel-format_, ...)`
  * `glBindTexture()`
  * `glTexImage2D()`
  * `glTexParameter*()`
