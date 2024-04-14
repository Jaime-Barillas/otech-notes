# Introduction To Modeling

Vectors:
+ It makes sense to transform points (positions)
+ It does not make sense to transform directions.

OpenGL user right handed coord system:
Y is up, X is to the right, z is out of the screen.

Winding order: CCW
determines which side is the inside, and which is outside.

Polygons must be planar for efficiency and simplicity. Given the equation for a
plane:

$$ ax + by + cz + d = 0 $$

The vector (a, b, c) is normal to the plane.

Meshes
store each vertex once and reference it per face.
Advantages:
+ Save memory, each vertex is stored once.
+ Each vertex is processed once -> faster
+ Helps reduce gaps in model
+ easily maps to OpenGL
Data is typically stored in 2 tables:

Vertex table
==========
+ location (pos)
+ normal
+ colour
+ etc.

Like an array buffer.

Face Table
=======
+ list of vertex indicies per face.

Like an element buffer.

Mesh Normals
+ Each vertex should have its own normal for smooth render.
+ Two steps: calc normal for each polygon face, then average all normals of a
  face a vertex is connected to.

Normal per Face
+ cross product of vectors that lie on plane of face.
+ vectors can be made from the vertices of each face.
+ Must construct them (use the verticies) in CCW order or the normals will have
  the wrong orientation.
+ Could cause problems if the vectors end up being near co-linear. Incredibly
  unlikely with triangles.
+ Polygons are easy to see with this method.

Normals per Vertex
+ As mentioned above
Efficient method:
table w/ 4 cols
normal x, normal y, normal z, count of polygons vertex is in.
loop through polygons and calc normal
add normal components to corresponding entry in table (i.e. for each vertex)
increment polygon count
divide all components by count
normalize.
done!
