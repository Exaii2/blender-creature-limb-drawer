# BLENDER-Creature-Limb-Drawer
![Limbdrawer_Header](https://user-images.githubusercontent.com/18192380/217829911-5eef2db8-1471-43b6-9941-7378dec11d2a.png)

A setup of geometry nodes and meshes, intended for prototyping creatures or preparing sculpts.

This project contains a blender scene, which consists mainly of a simple mesh containing only edges, and two collections for two different creature styles.

This addon is still in development and is currently developed using Blender 3.4.1. Previous Blender versions might also work, but were not tested.

I have created this personal tool to simplify my workflow for prototyping creatures. On top of that, I wanted the tool to be intuitive, usable no matter the blender expertise.
It could be compared to edge-based kitbashing, with similarities to the skin modifier.

# EXPLANATION

This tool mainly uses edges to create more complex geometry, similar to the skin-modifier.
Most of the magic happens in Geometry Nodes. Remeshing might help to reduce complexity.

I have created this tool to quickly prototype creatures. View it as a precursor for sculpting or perhaps creating indie type monster graphics.
Use the seeds in the geometry nodes to scramble which limb-parts are used, this can also be used to procedural generate more creatures from one base setup.

This tool is not a simple 'draw stick figures, get a sick creature'-geonodes setup (although that could work in some instances), it usually requires more than just one edge per limb to get better results. 
Overlayering multiple edges can increase volume of limbs while keeping an interesting surface.

# INTENDED WORKFLOW - Control structure

In the scene you have both a collection for 'LimbParts' and an Object called 'Control Structure'.
Most of the work happens in edit mode in the control structure.

![Hierarchy_1](https://user-images.githubusercontent.com/18192380/217834910-7ba84abb-3a1a-4c0e-ac35-62000789af4b.png)

Extrude single verts to create spikes or fingers. Extrude them once more to make the previous edge into a non-tip limb and create a new finger segment with the extruded vertice. 

![LimbDrawing_Example](https://user-images.githubusercontent.com/18192380/217837337-79f60028-bbc8-4bca-ad4e-a002dff970c2.png)

# INTENDED WORKFLOW - Limbparts

![LimbParts_Description](https://user-images.githubusercontent.com/18192380/217838260-c02895d8-7459-4832-9d83-1615be2c964c.png)

LimbParts describe the SubMeshs that are instanciated upon your edge control structure.
The length of edges uses different sub-meshes, which I call LimbParts. These can be changed in the geometry nodes - there are currently three limb-lengths for both LimbMiddle and LimbEnd. These feature different scalings which can be changed in the geometry nodes.

- LimbEnd: Any edge in which one vert has no other connecting edge. These usually are spikes, fingers, toes and talons.

- LimbMiddle: Any edge in which the connected verts have further connected edges. These can be used for both limbs and the main body, when overlayered multiple times.

In the scene there are corresponding collections for LimbEnd and LimbMid, sometimes containing more than one Mesh. 
Which mesh is used is based upon seeds in the Geometry Nodes. 
If you do not want to use randomized LimbParts, you can just clear the corresponding collection of unwanted LimbParts. If a LimbPart-collection is empty, the corresponding limb will not be instanciated.

# INTENDED WORKFLOW - Segments

![LimbPart_Detail](https://user-images.githubusercontent.com/18192380/217841709-7e35358b-f534-41df-adbc-9d34e0cea3d0.png)

Segments are small meshes in the MidPart- and EndPart-collection. They are low-polygonal structures with a mirror-modifier. Adjust these for different results. Note that the vertices are adjusted along the y-axis, -0.5 and 0.5 on the y-axis describe the from and to when instanciated by the goemetry nodes.

The complexity of the control structure mesh can, based on your rig, take up performance when live-editing. Especially when using the remesh-modifier.
Use simple shading for editing if performance is a problem.

This tool is mainly intended as a base for sculpting and to prototype creatures in a similar style. Adapt it and the parts here to your needs.

# INTENDED WORKFLOW - Geometry nodes

The geometry nodes have panels of different colors. Yellow indicates usage information, orange marks nodes that can be edited for varying final results. Red panels show issues or things to watch out for.

![Geometry Nodes Information](https://user-images.githubusercontent.com/18192380/217843049-99a5eee2-2ddf-4f22-8b65-5014348b37e3.png)

If you want to test different randomized LimbParts, you can change the corresponding seed in the geometry nodes instead. 
For versability, change the seeds to for each limbpart or the general seed which applies to all limbs. Change these in geometry nodes for immediate results or keyframe them and check through several iterations by using the timeline. 

![LimbPoint_Randomize](https://user-images.githubusercontent.com/18192380/217842118-998672d0-92cf-4e3f-8da9-4df2ff4f619a.png)

Also, you can adjust some scaling for the instanciated creature limbs.
In the LimbEnd/LimbMid Value frame, you can adjust the map-range node to modify the scaling for this limb-type.

![LimbPoint_1](https://user-images.githubusercontent.com/18192380/217843290-3f5399d3-efb9-4972-97b6-24150785dd69.png)

You can also change at the edge-length corresponding to certain limb-collections. The current edge-lengths are not adjusted for small-scale modelling. Scaling your control structure and applying the scaling will cause the geometry instances to update.

![LimbPoint_LengthValues](https://user-images.githubusercontent.com/18192380/217843809-90a0e588-6181-4e16-bf66-335f83cc7c94.png)

As the geometry nodes create lots of clipping meshes, one can also use the remesh modifier to reduce complexity. Applying a remesh modifier after the geometry nodes modifier on the control structure will create a smoother surface. Apply in order when you want to generate UVs or bake texture.
By my experience, voxel-based remesh tends to have smoothest results - you can also try to use Sharp with an level of about 8 and uncheck the 'remove disconnected' - this can create more worn-down results, especially at parts where the LimbParts overlap. 

Save your progress regularily. 

# CONTENT

This scene currently contains two different style of limbs: more technical and blocky mecha limbs, and the insect-like organic limbs. The mech-limbs are hidden.

Each can be used by setting the respective GeometryNodes modifier to one of the creature generator styles.

The geometry node setup are the same, except for the used collections for the LimbParts, the final material and some values for scaling. Currently, the used collections can be swapped manually only in the geometry nodes. You can skip the 'set material'-node, near the end of the geometry node setup.
The control structure meshes have a simple ambient occlusion based material. This requires rendering in cycles.

These base meshes are found in the respective collections. These meshes have only few detail.
You can drag the meshes into other parts of the collections. Having multiple meshes in one of these collection will lead to randomized LimbParts.

# CURRENT ISSUES AND LIMITATIONS

There are some known issues for this version. 

- MESH GENERATION

Currently, every LimbMid will also create a LimbEnd inside of it. The LimbEnd is usually not visible. This might change with future versions, but can also create more randomness in placed parts.

- LIMBEND ORIENTATION

Sometimes, when extruding endpoints, the resulting connected endpoint might be incorrectly aligned. I have yet to find a consistent solution for this. 
Be careful when subdividing edges, this can cause the ends to flip, too. Deleting endpoint vertices might cause Blender to update endpoints in unexpected ways. 

Extruding the vertex endpoint once (or sometimes twice) can usually fix most LimbEnd issues.

- LIMB ROTATION

The limb-orientation is based on the connected vertices. Moving the edge vertices will change the rotation to some degree. When trying to have correctly rotated fingers, you might need to adjust the rotation of the entire limb. This is certainly an trade off for accessability. 

- LIVE EDITING PERFORMANCE

Based on the power of your rig, you might want to stay in simple shaded for editing.
The base scene contains a deactivated remesh.
Editing the meshes of LimbMid and LimbEnd can require more perfomance when remesh is activated. If you only want to change the limbs, perhaps consider disconnecting the final geometry nodes to skip recalculations until your changes to the Limb-Meshes are finished.

- RIGGING

This tool only creates instanced geometry. Applying the modifiers of the control structure is recommended. You could use the skin modifier on the control structure with reduced vertices to create a base rig. 

# SUGGESTIONS AND FUTURE
You can contact me in my discord server (https://discord.gg/BJr6rT3Cer) for suggestions, troubleshooting and/or creations! 

At some point I might update this with more creature limb sets, maybe add an script to automatically set the collections via a master node.
