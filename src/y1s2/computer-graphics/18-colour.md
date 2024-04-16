# Color

## Two Vision Systems!!

1. Colour system under normal lighting conditions.
2. Black and White under low light conditions.

+ Both systems have detectors that give single value for all of the light they
  receive.
+ Computes some form of weighted average across 380 to 800 nm wavelength.
+ detector response is $k \overset{800}{\underset{380}{\int}} w(\lambda)L(\lambda)d\lambda$
  - $L(\lambda)$ is the amount of light reaching the detector.
  - $w(\lambda)$ is the weighting function.
  - The values of $w(\lambda)$ are determined experimentally.
+ The human colour vision system is based on three types of detectors called
  **cones**.
+ The values of $w(\lambda)$ overlap so various combinations could produce the
  same colour.
+ It is not a linearly dependant colour system.

## Colour

+ **Metamers** - two spectra (radiant power per wavelength) can produce the
  same colour.
+ RGB can't represent all colours. We would need negative colour (red) to make
  that possible.

## CIE

+ colour standard.
+ invents own 3 primary colours (X, Y, and Z).
  - Y is luminance.
  - X and Z is hue.
+ Not super easy to work with.
+ Normally projected onto $X + Y + Z = 1$ plane.
+ **Gamut** Range of colours available in a colour space.
+ Neither RGB nor XYZ are linear.
  - Be careful when interpolating (gradients), may not look smooth.
  - May perceive differences that aren't there when visualizing scalars.

## L\*U\*V* (and L\*a\*b)

+ Closer to perceptually linear.
+ Based on reference white colour ($X_n$, $Y_n$, $Z_n$), $Y_n$ is scaled to
  100.
+ Both use same value for L\*

## Colour Transforms

+ For any monitor there is a simple transformation from XYZ to RGB and back.
  - (a transformation exists for any two colour spaces.)
+ Treat RGB and XYZ as vectors and use a 3x3 matrix to transform them.
+ Same thing can be done with printers.
  - There are colours we can display on a monitor but not print.
