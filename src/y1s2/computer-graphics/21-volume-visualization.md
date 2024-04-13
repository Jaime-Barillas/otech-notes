# Volume Visualization

...

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
+ TODO

## Transfer Functions


## Illumination

Volume Visualization Slide 93
Recursive integral! An Integral equation.
