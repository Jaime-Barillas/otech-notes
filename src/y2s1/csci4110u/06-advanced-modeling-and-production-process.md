# Advanced Modeling

## Deformable Objects

+ Objects can change shape when forces are applied to it.
  - Can change quite radically.
+ Still have the same number of polygons connected into a mesh.
+ But polygons change shape.
+ A force is applied to the edge of a mesh or a point inside.
+ The force propagates to the other polygons within the mesh.
+ Example: Cloth.
+ Important for animation.
+ Objects with volumes must have their volume modeled.
  - Use a tetrahedron instead of a triangle.
  - The tetrahedron is the simplest volume primitve.
  - Conect many terahedron into a mesh-like structure.

## Water

+ Simulation is not real time.
+ Difficult to control.
+ Hard math.

## Machine Learning

+ Pretty recent in modeling.
+ Text to 3D is hard:
  - Models are 3D, not 2D images (simpler).
  - Very few training samples compared to 2D images.
+ Therefore, use a different approach:
  1. _DreamFusion_ - Text to model via images.
  1. _Coin3D_ - Start with simple geometry.

## NeRF

+ Neural Radiance Field.
+ A 3D grid of points with assigned values.
  - Could be Opacity or colour.
  - Occupancy.
  - Signed Distance Function.
+ For any realistic resolution, the grid will realy big.
  - Lots of memory.
  - Lots of processing time.
+ Solution: Replace the grid with a neural network.
  - Less space.
  - But requires training.
+ A NeRF can be specific to an object.
+ Can be parameterized to generate a range of objects.

## DreamFusion

+ Text to 3D.
+ Train a NeRF with many 2D images of an object.
  - Each image is generated using different camera positions and lighting positions.
+ Sometimes generates unrealistic objects.

## Coin3D

+ Start with rough 3D model.
+ Use text prompt to describe desired final model.
+ Generate 2D image candidates.
+ Apply chosen candidate to apply to rough model.
+ Generate final refined textured mesh.
+ Can be previewed in close to real time.
+ Can iteratively edit the model.
+ Supports editing the object onec it has been generated.
+ Produces better models with fewer artifacts.

## Humans

+ People are hard to model and simulate.
+ Hair is hard: lots of strands, each one can move.
  - Could try using a particle system.
+ Skin is hard.
  - Shape isn't too bad.
  - Rendering is hard.

## EyeNeRF

+ It exists.
+ Generates realistic eyes.

# Production Process

## The Production Team

+ There is no "I" in "Team".
+ Small studios have ~30 people.
+ Large animation teams have hundreds.
+ Each person will have their specialization.
  - Typicall divided into artists and programmers.

## Process

+ A typicall process involves:
  - Modeling.
  - Rigging.
  - Animation.
  - Composition.
  - Lighting.
  - Rendering.
  - Possibly more, these are the basics.
+ Many tools involved.
+ Programming most involved in Animation and Rendering.
+ At the start of the process we may have several different file formats.
  - At some point they will need to be converted to a standard format.
+ A fairly Waterfall like process.
  - If something changes at the beginning of the process, may need to re-run
    the whole thing.

## Asset Management

+ Need to keep track of everything produced.
+ Don't waste time duplicating assets.
  - e.g. For each character, have _one_ copy of their model, animations, etc.
+ Can end up being a lot of data.
+ Need to be able to find particular assets quickly.

## Tools

+ Each step has a set of tools.
+ Consider the file management problem:
  - Need one common format.
  - Solutions: gLTF or OpenUSD (or other).

## glTF

+ Common file format for 3D information.
+ Solves the transmission problem but not configuration management.
+ File format for all things needed to display 3D graphics.

## OpenUSD

+ More general than glTF.
+ Features to support all stages of the pipeline.
+ Plugin based.
  - Python and C++ API.
+ Open scene description with APIs for:
  - Creating.
  - Editing.
  - Querying.
  - Rendering.
  - Simulating.
  - etc.

## Modeling Tools

+ Used for geometrical modeling, rigging, animation.
+ Popular commercial modelers:
  - Maya.
  - Houdini.
  - These are expensive.
  - Steep learning curve.
+ Free modelers: Blender
  - Learning curve not as steep.
  - Geometrical modeling, rigging, animation, rendering, video editing.
  - Scripted using Python.

