# Visualization 2

+ Can't see black holes - dust absorbs visible wavelengths.
+ This around the black hole we can see at radio wavelengths.
  - Requires radio telescope, 1.3mm wavelength

## Black Holes

+ Bends space-time
+ Since space-time is curved, light rays bend near a black hole

**Schwarzschild radius**  
Non-rotating, non-charged black hole. Defines the event horizon.
$$
r_s = \frac{2GM}{c^2}
$$

$G$ is the gravitational constant. $M$ is the mass of the black hole. $c$ is
the speed of light.

**photon sphere**  
Sphere of zero thickness where photons orbit the black hole.
+ unstable, photons either escape or fall into the black hole.

Inner most stable orbit of black hole is at 3 times $r_s$. At greater distance,
you can safely orbit the black hole.

**accretion disk** - gasses spiralling into the black hole. They emit radiation
in the process. This is what we see of black holes.

## Visualization of M87*

+ From Earth M87* is small. It covers an angle of $40\mu as$
+ resolution of a radio telescope is proportional to the wavelength divided by
  its diameter
+ Would need a dish with a diameter the size of the Earth.
+ Can't do that (obviously.)
+ Instead build several telescopes at different locations.
  - If they are seperated by the diameter of the earth, they are almost as good
    as a telescope that large.
+ How does this work? Interference.
+ 2 telescopes pointed at same location recieve slightly different signals due to
  different distances to object.
+ Led to EHT

## EHT - Event Horizon Telescope

+ network of telescopes
+ cover diameter of Earth
+ each telescope has an atomic clock, synchronized
+ data recorded during observation saved to disk + timestamp
+ sent to MIT for processing
+ data is raw signals from pairs of telescopes, not an image.
+ what does a correct visualization look like? (no one's seen a black hole
  before.)

## Algorithms

1. **CLEAN** - start with data -> compute pixel values.
2. **RML** - start with pixel values -> use optimization process to match the
   pixel values to the data.

+ multi-step process was used to build visualization
+ Step one:
  - take 4 independent teams, no communication allowed
  - multiple weeks to build their pictures
  - results were similar not too bad
  - allowed estimation of black hole properties
  - produced approximation of good parameters for visualization techniques.
+ Step two:
  - use math models w/ estimations to simulate what it should look like.
  - four possible geometries chosen and modeled
  - EHT signals were computed for the chosen geometries
  - the new data was used to find better parameters for the algos
+ Step three:
  - introduce additional simulation to ensure overfitting of data did not ocurr.

Or just see slide 32.

## Simulation

+ Einstein developed equations of general relativity.
+ Others developed equations for black holes.
+ Maths understood by 1930s but visualizations came 40 years later.
+ Equations are complicated.
+ Couldn't do it until sufficiently powerful computers existed.
+ We would see the accretion disk or stars behind it, not the black hole itself.
+ Use _ray tracing_.
+ rays do not follow straight line near black hole
  - compute the paths
  - if intersecting a star, work illumination back along the curved path to the
    viewer
+ ray tracing previously ignored what light was made of.
+ now, light consists of particles which interact with gravitational field.
+ observer is likely moving at high speed around black hole.
+ How to handle this?

## Approach

+ develop differential equations for path of photons starting at observer
  - non-trivial equations
  - solved numerically
  - compute path photon would take
+ **Termination Conditions**:
  - Photon intersects star or accretion disk
  - Photon goes to infinity w/out intersection
  - Photon absorbed by block hole
+ starts could be very far away, long path to trace

### Star Intersection

+ map the stars onto large sphere enclosing observer and black hole:
  **celestial sphere**.
+ intersect ray w/ sphere to see if it strikes a star
+ stars are point light sources, rays infinitely thin, almost impossible to
  have intersection.
  - assume ray has diameter, look for intersections w/in diameter.
+ one star is intersected, propegate light back to observer
  - requires second set of differential equations that model light transport
  - gravitational field effects the frequency of light, could shift it red or
    blue
+ star light can appear in multiple positions
+ computed path can circle the black hole multiple times

### Interstellar (Movie)

+ film needed high quality image, physics simulations aliased badly.
+ instead of ray tracing, trace light beams (give ray some volume.)
+ beam starts as circle at camera, projects as ellipse on celestial sphere.
+ stars w/in ellipse averaged together -> stars no longer pop between frames.
