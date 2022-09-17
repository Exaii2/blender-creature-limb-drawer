# BLENDER-Creature-Limb-Drawer

An blender scene that generates geometry by drawing edges. Intended for prototyping creatures or preparing sculpts.

This project contains a blender scene, which consists mainly of a simple mesh containing only edges, and two collections for two different creature styles.

This addon is still in development and was created with Blender 3.2. (to this date in Alpha). Previous Blender version might also work, but are not tested for the moment.

I wanted to create a tool that simplifies my workflow to prototype creatures, no matter the expertise level.
So far I have not seen anything that comes close. If there are similarities to a project or addon of yours, these are not intended. 

!--HEADER PICTURE

# EXPLANATION

This tool mainly uses edges to create more complex geometry. 
Most of the magic happens in Geometry Nodes. A remesh might help to reduce complexity.

I have created this tool to quickly prototype creatures. View it as a precursor for sculpting or perhaps creating indie type monster graphics.
Use the seeds in the geometry nodes to scramble which limb-parts are used, this can also be used to procedural generate more creatures from one base setup.

This tool is not a simple 'draw stick figures, get a sick creature'-geonodes setup (although that could work in some instances), it usually requires more than just one edge per limb to get better results. 
Overlayering multiple edges can increase volume of limbs while keeping an interesting surface.

# INTENDED WORKFLOW

Most of the work happens in edit mode of the creature. Extrude single verts to create spikes or fingers. Extrude them once more to make the previous edge into a non-tip limb and create a new finger segment with the extruded vertice.
Edges that have a single vert that acts as an endpoint will create fingers, talons or spikes.

The length of edges uses different sub-meshes. These can be changed in the geometry nodes - there are currently three limb-lengths for both LimbMiddle and LimbEnd. These feature different scalings which can be changed in the geometry nodes.
LimbEnd: Any edge in which one vert has no other connecting edge. These can be spikes, fingers, toes and talons.
LimbMiddle: Any edge in which the connected verts have further connected edges. These can be used for both limbs and the main body, when overlayered multiple times.

!--LIMBMID / LIMBEND EXPLANATION HERE

For versability, change the seeds to for each limbpart or the general seed which applies to all limbs. Change these in geometry nodes for immediate results or keyframe them and check through several iterations by using the timeline. 

!--SEEDS IN GEONODES

I recommend use simple shading for editing. I have tested this on a good rig, however live editing with complex shaders can lead to FPS loss. The GeoNodes with the used Remesh can slow down when increasing complexity.

This tool is mainly intended as a base for sculpting and to prototype creatures in a similar style. Adapt it to your needs.

As the geometry nodes create lots of overlapping meshes, you might want to use a remesh modifier to create a smoother surface and reduce artifacts.
Once expecations are met, apply the modifiers (and perhaps keep a copy of your Creature-Edge-Setup).

The geometry nodes have panels of different colors. Yellow indicates usage information, orange marks nodes that can be edited for varying final results. Red panels show issues or things to watch out for.

# CURRENT ISSUES AND LIMITATIONS

There are some known issues for this version. 

- MESH GENERATION

Currently, every LimbMid will also create a LimbEnd inside of it. The LimbEnd is usually not visible. This might change with future versions, based on available nodes.
!--LIMBENDS IN LIMBMIDS

- LIMBEND DIRECTION

Sometimes, when extruding endpoints, the resulting connected endpoint might be incorrectly aligned. I have yet to find a consistent solution for this. 
Be careful when subdividing edges, this can cause the ends to flip, too. 
Extruding the vertex endpoint once or twice more can fix this.

The optimal way of creating LimbEnds by my experience:
!--ENDPOINT DIRECTIONAL ISSUE

- LIMB ORIENTATION

The limb-orientation is based on the connected vertices. Moving the edge vertices will change the rotation to some degree.

- LIVE EDITING PERFORMANCE

Based on the power of your rig, you might want to stay in simple shaded for editing.
The base scene contains a deactivated remesh.
Editing the meshes of LimbMid and LimbEnd can require more perfomance when remesh is activated. If you only want to change the limbs, perhaps consider disconnecting the final geometry nodes to skip recalculations until your changes to the Limb-Meshes are finished.

- RIGGING

This tool only creates geometry. You could use the skin modifier with reduced vertices to create a base rig.

# SUGGESTIONS AND FUTURE
You can contact me in my discord for suggestions and troubleshooting. I am just starting with this project.

Please do NOT contact me in regards to NFT-projects.

For my artworks I also use my own creature limb set and my own shaders. These are not contained in the project files. 
At some point I might update this with more creature limb sets.
