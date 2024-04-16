# Volume Visualization

+ 3D dataset.
+ Approach 1: View data as 3D texture.
  - Extraction step program produces textures.
  - Display step program displays the textures.

## Simple Display Method

Insert plane into 3D space of texture and use its coords as the texture
coordinates. Load images into a `GL_TEXTURE_3D` via `glTexImage3D`.

## Volume Rendering

+ produce visualization from 3D uniform rectilinear grid of scalars
+ see more than one voxel at each point on the screen
+ all techniques can be divided into two parts:
  - Determine voxels (or points) that contribute to each pixel on the screen
  - Determine colour of pixels as a function of the corresponding voxels
+ **Transfer Function** - function/method used to determine the pixel colour.
  Usually a function of the current pixel colour and voxel, and sometimes the
  path from the pixel to the voxel.
+ two methods for determining the voxels that contribute to each pixel value:
  - Start with the pixel and search through the volume for voxels
  - Start with the voxel and determine the pixels that it maps too

Assume Orthographic projection:

### Start With Pixel, Search For Voxels

+ Build ray.
+ Start at pixel center.
+ Move perpendicular to screen.
+ Sample volume at values along ray path using the **transfer function**.
  - Sample uniformly - more expensive, better results.
  - Sample volume at each voxel - less expensive, not as good results.

#### Sampling

Uniformly:
+ Find where ray enters and leaves the volume.
+ Divide that segment into a number of uniform length steps.
+ Sample the volume at each step along the ray.
+ May need to interpolate data points since step location likely won't
  coincide w/ data points.
  - Can choose closest data point.
  - Can interpolate (8 points since each voxel has 8 corners.)
+ Step size can't be too large (expensive.)
+ A small step size misses important features.

Voxels:
+ Don't need to worry about interpolation.
+ Less worry about step size.
+ Path of ray is no longer straight line. Jumps from voxel to voxel.
+ What is the "next" voxel?
  - 3 possibilities.
  - **6 connected** - One of the voxels sharing a face with the current one.
  - **18 connected** - Voxel sharing face or edge.
  - **24 connected** - Voxel sharing face, edge, or vertex.
  - 18 connected is a good compromise most of the time.
+ Can be optimized w/ orthographic projection.
+ Rays would take parallel paths through volume.
  - Path can be translated by appropriate amount for each pixel.
+ Works best if view plane is parallel to a side of the volume.
+ May miss voxels or not get uniform sampling if volume is not parallel to view
  plane.
+ A partial solution is to start ray paths at the surface of the volume.
  - The resulting image will have to be transformed onto the viewing plane.
+ Can also start with voxels in the volume and find pixels they map to.
  - Must start at back of volume and work forwards.

## Transfer Functions

+ Provides colour of each pixel.
+ Sampled at each point along ray.
+ Can be function of:
  - current pixel value.
  - voxel value.
  - distance to voxel.
  - gradient at voxel.
  - etc.
+ **Maximum/Minimum** - Takes the max/min value along the ray.
+ **Average** - Computes the average voxel value along the ray.
+ **Distance to Value** - Computes the distance to a particular voxel value.
+ **Composite** - Considers the voxel value to be opacity. Accumulates opacity
  along the ray.

+ Opacity gives us a way of seperating structures within the volume.
+ Works well when each structure has a well defined range of values that don't
  overlap.
+ If there is overlap, we need additional information.
  - Where do we get this?
  - Could try the magnitude of the gradient vector.
  - i.e. The voxel value may not be the best indicator of different structures,
    but the rate of change of voxel values _might_ be.

## Illumination

Should we apply lighting or illumination to our volume rendering?

+ Could be a bad idea (in e.g. medical applications.)
  - alters colours, could alter interpretation.
+ Could help interpret 3D structure.

To illuminate:
+ Could try to estimate normal vector from gradient and info from adjacent
  voxels.
+ Could use **transport theory**, a model of how light is scattered and
  absorbed in a volume.
  - Requires recursive integral.
  - big math, big hard.
  - approximations have been developed, but look like the composition function
    in their simplest case.
