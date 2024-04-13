# The Graphics Pipeline

## LCD Displays
"Liquid Crystal Displays" - large molecules that change their optical
properties when an electrical field is applied to them.
+ Many different types of LCDs.
+ LCDs are made up of many small cells containing these liquid crystals.
+ LCDs are a rectangular array of these cells with small wires running through
  the cells to apply the electrical field.
+ The size of the array (width and height) is called the **resolution** of the
  display.
+ Each cell is a pixel.

+ Each cell is capable of displaying one colour.
+ We display an image by specifying the colour of each pixel on the screen.
+ Approx 1-2 Million pixels, so this is a lot of information to specify.
+ Fixed resolution, CRT's could handle a range of resolutions.

## Rasters
+ The graphics card has a 2D array of memory with one entry for each pixel.
+ The entry is typically the colour of the pixel.
+ This array is called a **framebuffer**.

## Representing Colour
+ Human response to light intensity is logarithmic not linear.
+ This impacts display design.
+ Several different wavelength of light can produce the same colour.
+ We approximate colour based on the RGB colour space.
+ The total colour space is called a gamut (range of colours that can be
  represented.)
+ RGB is a close match to the colours produced by monitors, so it is used in
  computer graphics.
+ **Note:** We cannot use a primary based system to get _all_ colours, unless
  we allow _negative colours_.

## Sending the Pixels
+ Need an organized way of getting the pixels onto the screen (The info carried
  by VGA/HDMI/etc. protocols).
+ Use a **Scanning Pattern**. The pixels are sent one at a time in some order.
+ Typically: start with the pixel in the top left corner and go pixel by pixel,
  row by row.
+ Once the bottom row is reached, start over.
+ At the end of each line and screen, there is a small amount of time that is
  used by CRTs to return their beam to the start of the line or the top of the
  screen (this is called **horizontal** and **vertical retrace**.)
+ We also need a **synchronization signal** to tell the monitor when we are at
  the top of the screen and when we are about to start each line.

## Refresh Rate
The graphics card handles this, we don't need to worry about it in terms of
hardware.

## Rendering
+ The process of converting geometrical information into pixels in the
  framebuffer.
+ Can be real-time or non real-time.
+ Techniques such as Global Illumination are for non real-time rendering.
+ OpenGL focuses on real-time rendering.

## The Graphics Pipeline

### Models
+ Representation of an object.
+ Usually mathematical in nature.
+ Made up of triangles.
+ Each triangle has 3 associated vertices.
+ Models may have millions of triangles.

> We currently have 2 parts of the Graphics Pipeline:
> 1) The model - triangles
> 2) The display - pixels

The graphics pipeline tries to efficiently convert triangles to pixels.

+ Need to project 3D triangles to a 2D image.
+ Use a view transformation (perspective):
  * type: perspective.
  * eye location needed.
  * viewing direction needed.
  * up direction needed.
+ Perspective projection defiens a **viewing frustum**.
+ The viewing frustum essentially clips all the triangles.
+ If triangles are not clipped they will appear in incorret positions.
+ The process of eliminating the triangles is called **clipping**.

### Local and Global Axes

**Local** - Local to the model.  
**Global** - "Local" to the "world".

### Adding Colour to Triangles
We do this by defining a **lighting model** and **material properties** for our
models.

+ Lights have a position.
+ Lights have a direction.
+ Lights can have a colour.
+ We need to assign material properties to the light: How much of the light
  gets reflected and how.
+ Normal vectors determine how light is reflected off of a _triangle_ surface.
+ Vertecies get a normal vector that is the average of the normal vectors of
  the triangles it is connected to.

### Shading

**Gouraud Shading** use the material properties, vertex position, and vertex
normal to calculate the colour of the vertex and interpolate it accross the
triangels of the model.

**Phong Shading** instead interpolates the _normal vector_ across the surface
and computes the colour at each pixel.

The current stages of the graphics pipeline. For each triangle:
> Transform the vertices into global "world" coordinates.
> Project the vertices.
> Clip the triangles to the viewing frustum.

### Pixel Filling
**Filling** is determining the pixels inside of a triangle's outline.
+ There are efficient algorithms for this implemented in GPU hardware.
+ As we fill each pixel we compute its colour as mentioned above (Phong style.)
+ Issue: What about triangles behind or infront of other triangles?
+ We don't want to draw triangles behind others. This is called the **Hidden
  Surface Problem**.
+ Current graphics cards solve it in hardware. We use a depth buffer.
+ The depth buffer holds the "depth" of the corresponding pixel.
+ If a pixel is potentially closer to the viewer (because a triangle is in
  front of another one), draw that pixel and update the depth buffer.
+ The x, y, z coords of an entire triangle (including pixels on its surface)
  are interpolated from the vertices.
+ For our course pixels are also called fragments.

## Overall Pipeline Steps
> Geometrical Proccessing:
> Transform the vertices into global "world" coordinates.
> Project the vertices.
> Clip the triangles to the viewing frustum.
> Fragment Processing:
> Compute the pixels covered by the triangle.
> Compute the colour of the pixel.
> Interpolate the depth value.
> Deal with hidden surfaces using the Z buffer.
