# BLENDER-Creature-Limb-Drawer

![DifferentStyles](https://user-images.githubusercontent.com/18192380/226145510-201b7f66-ed1a-4a95-8f38-bb519a1cdc44.png)

A setup of geometry nodes and meshes, intended for prototyping creatures or preparing sculpts.

This project contains a blender scene, with two prepared control structures, to which different styles can easily be applied.
To check out the different styles, use either shift and the numbers 1-6 in object mode to quickly enable the collections or manually enable these from the hierarchy.
Some styles use more parts than others and thus utilize random seeds. These can be adjusted quickly from the modifier stack.

This addon is still in development and is currently developed using Blender 3.4.1. Previous Blender versions might also work, but were not tested.

I have created this personal tool to simplify my workflow for prototyping creatures. On top of that, I wanted the tool to be intuitive, usable no matter the blender expertise.
It could be compared to edge-based kitbashing, with similarities to the skin modifier.

# EXPLANATION

This tool uses edges to create more complex geometry, similar to the skin-modifier. Instead of projecting a cube along the edges, it will project meshes from prepared collections, the selection is based on the length of the edge.
Most of the magic happens in Geometry Nodes. Remeshing might help to reduce complexity.

I have created this personal setup to quickly prototype creatures. View it as a precursor for sculpting or perhaps creating indie type monster graphics.
Use the seeds in the geometry nodes to scramble which limb-parts are used, this can also be used to procedural generate more creatures from one base setup.

This tool is not a simple 'draw stick figures, get a sick creature'-geonodes setup (although that could work in some instances), it usually requires more than just one edge per limb to get satisfying results. 
Overlayering multiple edges can increase volume of limbs while keeping an interesting surface.

# INTENDED WORKFLOW - Control structure

In the scene, for each style there are multiple collections for 'LimbParts' and two meshes, here called 'Control Structure'. The infoplane is only a visual aid and can be disregarded.

![NamedCollections](https://user-images.githubusercontent.com/18192380/226145489-44ee2f15-767b-4dfe-857e-a91963250046.png)

Image 1: Current naming convention for collections.

To modify one of the prepared control structure, use the edit mode and perhaps enable X-Ray in the menu bar. 

![X-Ray](https://user-images.githubusercontent.com/18192380/226145461-75447a42-63ff-4c66-8cdd-36820798ae5c.png)

Image 2: X-Ray can help finding editing the vertices, while the style GeoNodes are active.

Extrude single verts to create spikes or fingers. Extrude them once more to make the previous edge into a non-tip limb and create a new finger segment with the extruded vertice. 

![LimbDrawing_Example](https://user-images.githubusercontent.com/18192380/217837337-79f60028-bbc8-4bca-ad4e-a002dff970c2.png)

Image 3: Comparison between generated mesh and underlying control structure.

# INTENDED WORKFLOW - Limbparts

The prepared naming convention have changed since Version 1.08.

![HierarchyNamingConvention](https://user-images.githubusercontent.com/18192380/226145454-48e0d7bb-8079-43db-b1a9-3bb03daddd2a.png)

Image 4: Naming convention in hierarchy.

LimbParts contains multiple collections, which are instanciated upon the edge control structure.
Based on the length of an edge, either a mesh from the small, medium or large collection is instanced. The length at which each of these collection is used, can be controlled by two values in the GeometryNodes.

Additionally, for each of the three length-types, there are also LE- and LM-Collections.

- LE/LimbEnd: Any edge in which one vert has no other connecting edge. These usually are spikes, fingers, toes and talons.
- LM/LimbMiddle: Any edge in which the connected verts have further connected edges. These can be used for both limbs and the main body, when overlayered multiple times.

Overall, each style uses six (possibly different) collections.
Should a collection contain more than one mesh, each edge will chose one by a random seed. This seed can be adjusted in the modifier stack.

# INTENDED WORKFLOW - Segments

![Segment_Detail](https://user-images.githubusercontent.com/18192380/226145434-4f080cf4-a5b9-4a97-90ad-13196c4b8848.png)

Image 5: Multiple Segments for LimbMedium-Large (organic style)

Segments are small meshes in the MidPart- and EndPart-collection. They are low-polygonal structures, sometimes with a mirror-modifier. Adjust the meshes for different results. 
Note that the meshes are adjusted along their local y-axis, usually between -0.5 and 0.5. This y-range will ensure that the instance will be placed correctly along the edge control structure.

The complexity of the control structure mesh can, based on your rig, take up performance when live-editing. Especially when using the remesh-modifier.
Use simple shading for editing if performance is a problem.

This tool is mainly intended as a base for sculpting and to prototype creatures in a similar style. Adapt it and the parts here to your needs.

# INTENDED WORKFLOW - Geometry nodes

The GeometryNodes for the overarching styles are simplified since Version 1.08.

![6_SimplifiedGenericLimbGenerator](https://user-images.githubusercontent.com/18192380/226146677-98df0f86-6b5c-4f8c-a96a-684646892159.png)

Image 6: Simplified NodeGroup in GeometryNodes.

Feel free to look into the groups (like Generic Creature Limb Generator) in case you want to manually adjust the behaviour of this tool.
The 'delete geometry'-node at the beginning is there to prevent weird behaviour when using faces.
Compare different styles to see how one can utilize the 'Generic Creature Limb Generator'-Node.

- Seeds

When using a creature limb style which has multiple meshes per LimbPart-Collection, you can utilize the Seeds for both MidLimbs and EndLimbs.

![ModifierStack_Seeds](https://user-images.githubusercontent.com/18192380/226145656-e2681185-2320-4265-8c7a-8a3ec3c368f0.png)

Image 7: Adjustable seeds in the modifier stack.

Change these in the modifier stack for immediate results or keyframe them and check through several iterations by using the timeline. 

- XY-Scaling

The XY-scalings for small, medium and large limb-parts can also be adjusted in the GeometryNodes for that style.
Modify the vector-values, which act as min-size/max-size for both EndLimbs and MidLimbs. X, Y and Z are affecting the XY-Scale of small, medium and large limbs, respectively.

![XYSCaling](https://user-images.githubusercontent.com/18192380/226145404-3754c575-22fe-4a12-9a0d-c88087696448.png)

Image 8: Adjustable scaling in GeometryNodes.

The 'XY-Scale'-Value below these four vectors can be changed to make all limbs bigger/smaller at once. Some of the styles already have a slight number adjustments, play around with these to see the effect.

- LimbLength

The length at which a small limb turns into a medium or large limb is adjustable.

![LimbLength](https://user-images.githubusercontent.com/18192380/226145369-f50aeccf-08af-4c19-8133-6f19f5c166a2.png)

Image 9: The LimbLength-values in GeometryNodes.

Note that scaling the control structure and applying the scaling will cause the geometry instances to update, which will change the overall generated mesh.

- LimbEnds in LimbMids

There is an option to generate LimbEnds into every LimbMid. This was at first unintended, but also resulted into more randomness, especially when using organic shapes.
This effect can be re-enabled by using the Boolean 'Generate LimbEnds in LimbMids' inside the GeometryNodes. 

![GenerateLimbEndsInLimbMids](https://user-images.githubusercontent.com/18192380/226145327-c13fb831-499e-41d2-a1e3-870e27c9d70d.png)

Image 10: The marked Bool-option in GeometryNodes.

- Polishing the mesh

As the geometry nodes can create clipping meshes, the remesh modifier might help to reduce complexity. 
Applying a remesh modifier after the geometry nodes modifier on the control structure will create a smoother surface. Apply in order when you want to generate UVs or bake texture.
By my experience, voxel-based remesh tends to have smoothest results - you can also try to use Sharp with an level of about 8 and uncheck the 'remove disconnected' - this can create more worn-down results, especially at parts where the LimbParts overlap. 
When using a remesh, your mesh will lose the material data from the instances. When using only a single material, using an new GeoNodes-Setup with only a 'Set Material'-Node might fix this.

Save your progress regularily. 

# CONTENT

This scene currently contains five different style of limbs: 
Switch between those in Object Mode and using 'shift and the numbers 1 to 6' to easily preview these. 

![DifferentStyles](https://user-images.githubusercontent.com/18192380/226145326-24eee729-3f3d-439c-b667-22af36928dbc.png)

Image 11: Different Styles Showcase, as shown in order below

The current official styles are:
- Mech: technical, blocky parts.
- Organic: a selection of shell-like parts.
- CyberneticSaw: edgy technical parts.
- StylizedRoot: lowpoly root-like structures.
- Prosthesis: an experimental, very simple style for limbs. Might work better when not overlayered.

Each can be used by setting the respective GeometryNodes modifier to one of the 'creature limb generator (style)'.

These control structure meshes are found in every style collection. These are duplicates and only differ in which geometry node setup is used. These meshes do not have a set material themselves.

The instanced LimbPart-meshes have a simple ambient occlusion based material applied to them. These require rendering in cycles for correct display.
Consider using a 'set material'-node in the GeometryNodes after the 'Generic Creature Limb Generator'-node, if you quickly want to swap out the material for the whole generated mesh.

# CURRENT ISSUES AND LIMITATIONS

There are some known issues for this version. 

- LIMBEND ORIENTATION

Sometimes, when extruding endpoints, the resulting connected endpoint might be incorrectly aligned. I have yet to find a consistent solution for this. 
Be careful when subdividing edges, this can cause the ends to flip, too. Deleting endpoint vertices might cause Blender to update endpoints in unexpected ways. 
Extruding the vertex endpoint once (or sometimes twice) can usually fix most LimEnd issues.

- LIMB ROTATION

The limb-orientation is based on the connected vertices. Moving the edge vertices will change the rotation to some degree. When trying to have correctly rotated fingers, you might need to adjust the rotation of the entire limb. 
This is certainly an trade off for accessability. 

- LIVE EDITING PERFORMANCE

Based on the power of your rig, you might want to stay in simple shaded for editing.
The base scene contains a deactivated remesh.
Editing the meshes of LimbMid and LimbEnd can require more perfomance when remesh is activated. If you only want to change the limbs, perhaps consider disconnecting the final geometry nodes to skip recalculations until your changes to the Limb-Meshes are finished.

- RIGGING

This tool only creates instanced geometry. Applying the modifiers of the control structure is recommended. You could use the skin modifier on the control structure with reduced vertices to create a base rig. 

# SUGGESTIONS AND FUTURE
You can contact me in my discord server (https://discord.gg/BJr6rT3Cer) for suggestions, troubleshooting and/or creations! 
