# BLENDER-Creature-Limb-Drawer

![0-1_Header](https://user-images.githubusercontent.com/18192380/236580383-9a9e8117-4f1f-4747-91e2-5934bd7d7653.png)

Image 0-1: Main Header

![0-2_CreatureLimbDrawer_MainShowCase](https://user-images.githubusercontent.com/18192380/236580393-82c8cd7a-3284-4ef6-9f40-f9d5072841d9.gif)

GIF 0-2 : GIF: Creature Limb Drawer in Action

A setup of geometry nodes and meshes, intended for prototyping creatures or preparing sculpts.

This project contains a blender scene, with two prepared control structures, to which different styles can easily be applied.
To check out the different styles, use either shift and the numbers 1-7 in object mode to quickly enable the collections or manually enable these from the hierarchy.
Some styles use more parts than others and thus utilize random seeds. These can be adjusted quickly from the modifier stack.

This addon is developed using Blender 3.6 (since the 1.26 version). 
Previous Blender versions might also work, but were not tested.
For older Blender-versions, check previous commits.

I have created this personal tool to simplify my workflow for prototyping creatures. It could be compared to edge-based kitbashing, with similarities to the skin modifier.

# 1 EXPLANATION

This tool uses edges to create more complex geometry, similar to the skin-modifier. Instead of projecting a cube along the edges, it will project meshes from prepared collections, the selection is based on the length of the edge.
Most of the magic happens in Geometry Nodes. Remeshing might help to reduce complexity.

I have created this personal setup to quickly prototype creatures. View it as a precursor for sculpting or perhaps creating indie type monster graphics.
Use the seeds in the geometry nodes to scramble which limb-parts are used, this can also be used to procedural generate more creatures from one base setup.

This tool is not a simple 'draw stick figures, get a sick creature'-geonodes setup (although that could work in some instances), it usually requires more than just one edge per limb to get satisfying results. 
Overlayering multiple edges can increase volume of limbs while keeping an interesting surface.

The Asset Browser can be used since the update to the 1.20 version. 
With the Asset Browser, you can drop the described effects onto any mesh you like without opening the main file. More on that at 2.4/2.5. 

With the 1.26 version, limb rotation has been made more consistent.

# 2.1 INTENDED WORKFLOW - Control structure

In the scene, each style has are multiple collections for 'LimbParts' and some display meshes, here called 'Control Structure'. The infoplane is only a visual aid and can be disregarded.

![2 1-1_NamingConvention-Collections](https://user-images.githubusercontent.com/18192380/236580432-9eaba1b6-e3a4-45db-a7ab-ef07651092aa.png)

Image 2.1-1: Current naming convention for collections.

- X-Ray

To modify one of the prepared control structure, use the edit mode and perhaps enable X-Ray in the menu bar. 

![2 1-2_X-Ray](https://user-images.githubusercontent.com/18192380/236580451-29f494d8-23a1-4f83-a700-58a7a3a6c115.png)

Image 2.1-2: X-Ray helps while editing the vertices with the active GeoNodes-modifier.

- General usage

Extrude single verts to create spikes or fingers. Extrude them once more to make the previous edge into a non-tip limb and create a new finger segment with the extruded vertice. 

![2 1-3_LimbDrawing_Example](https://user-images.githubusercontent.com/18192380/236580457-d22a8b18-e0d2-411f-b778-a5bc68199c06.png)

Image 2.1-3: Comparison between generated mesh and underlying control structure.

![2 1-4_CreatureLimbDrawer_Extrude](https://user-images.githubusercontent.com/18192380/236580470-9f68e4e0-c6b0-4cb9-ad0a-2999d687c493.gif)

GIF 2.1-4: GIF: Extruding parts, creating limbs.

- Rotational reference

Added in the 1.20 version, you can now add an object as a rotational reference in the geometry nodes-modifier.
When one is added (and not placed at the world-zero), this will affect all the internal rotations of limbparts.
For examples, see the biped-arms or the quad-walker. You can set the mesh-object itself as a reference, resulting in all limbs to rotate towards the origin.

![2 1-7_RotationalReference](https://user-images.githubusercontent.com/18192380/236580513-06fd6bb9-3330-4268-b92d-dde10fe5e057.png)

Image 2.1-7: Rotational Reference in the Modifier Stack

![2 1-8_CreatureLimbDrawer_RotationalObject](https://user-images.githubusercontent.com/18192380/236580521-82d2b21a-72ee-4df6-b170-913d948be3c7.gif)

GIF 2.1-8: GIF of rotational reference in action.

# 2.2 INTENDED WORKFLOW - Limbparts

The naming convention has been changed since the 1.08 version.

![2 2-1_HierarchyNamingConvention](https://user-images.githubusercontent.com/18192380/236580543-3fe69bf5-0ea3-4022-9eb8-cef78e58f084.png)

Image 2.2-1: Naming convention in hierarchy.

LimbParts contains multiple collections, which are instanciated upon the edge control structure.
Based on the length of an edge, either a mesh from the small, medium or large collection is instanced. The length at which each of these collection is used, can be controlled by two values in the GeometryNodes.

Additionally, for each of the three length-types, there are also LE- and LM-Collections.

- LE/LimbEnd: Any edge in which one vert has no other connecting edge. These usually are spikes, fingers, toes and talons.
- LM/LimbMiddle: Any edge in which the connected verts have further connected edges. These can be used for both limbs and the main body, when overlayered multiple times.

Overall, each style uses six (possibly different) collections.
Should a collection contain more than one mesh, each edge will chose one by a random seed. This seed can be adjusted in the modifier stack.

# 2.3 INTENDED WORKFLOW - Segments

![2 3-1_Segment_Detail](https://user-images.githubusercontent.com/18192380/236580555-9bd9f1e7-4f80-4b40-83e5-2ad02d225e48.png)

Image 2.3-1: Multiple Segments for LimbMid-Large (Organic style)

Segments are small meshes in the MidPart- and EndPart-collection. They are low-polygonal structures, sometimes with a mirror-modifier. Adjust the meshes for different results. 
Note that the meshes are adjusted along their local y-axis, usually between -0.5 and 0.5. This y-range will ensure that the instance will be placed correctly along the edge control structure.

The complexity of the control structure mesh can, based on your rig, take up performance when live-editing. Especially when using the remesh-modifier.
Use simple shading for editing if performance is a problem.

This tool is mainly intended as a base for sculpting and to prototype creatures in a similar style. Adapt it and the parts here to your needs.

# 2.4 INTENDED WORKFLOW - Asset browser

As seen in the initial GIF. 
For easy usage, place this file in your Asset Bundle-folder. 
- If you do not have one set, in the main menu bar, click on 'Edit > Preferences' and navigate to the 'File Paths'-tab. 
- Scroll towards 'Asset Libraries', click the '+'. 
Set the folder in your system that contains the 'Creature Limb Drawer'-blend-file.

![2 4-1_Preferences-AssetBrowser_marked](https://user-images.githubusercontent.com/18192380/236580586-39d305d9-6fba-48f2-9c96-b69c83df9583.png)

Image 2.4-1: Preferences

Afterwards, set an Area to Asset Browser. You should now be able to see the groups.

![2 4-2_AssetBrowser](https://user-images.githubusercontent.com/18192380/236580618-645fe5fa-c64c-4093-99e2-efde06d6263c.png)

Image 2.4-2: Asset browser in action

# 2.5 INTENDED WORKFLOW - new set for the assetbrowser

If you wish to create your own Limb-Set for the Asset Browser with a fitting thumbnail, follow this description.
Use the seperate 'CreatureLimbDrawer_Your Style'-blend-file and rename it to your wishes. Keep _asset_bundle at the end. 
The file should be contained within your set Asset Browser-folder. See 2.4.

- 1
Rename the 'Limb Gen (STYLE-NAME)'-Geometry Nodes to your liking. 
Rename all subcollections to fit the new style name.

- 2
Modify the segment-meshes in the subcollections however you see fit. 
Duplicate meshes inside the collection if you want a random selection for that length range of a limb.
Set the materials of the limbs.

- 3
For a fitting thumbnail, just hit render.
Render the image and save it.
In the Asset Browser, find your new limbstyle, and set the preview to your rendered image. 

# 2.6 INTENDED WORKFLOW - Geometry nodes

The GeometryNodes for the overarching styles are simplified since Version 1.08.

![2 6-1_SimplifiedGenericLimbGenerator](https://user-images.githubusercontent.com/18192380/236580639-dfdc0cb2-3b1c-4ab2-8341-cd4ce61c629f.png)

Image 2.6-1: Simplified NodeGroup in GeometryNodes.

Feel free to look into the groups (like Generic Creature Limb Generator) in case you want to manually adjust the behaviour of this tool.
The 'delete geometry'-node at the beginning is there to prevent unforseen behaviour when using faces.
Compare different styles to see how one can utilize the 'Generic Creature Limb Generator'-Node.

- Seeds

When using a creature limb style which has multiple meshes per LimbPart-Collection, you can utilize the Seeds for both MidLimbs and EndLimbs.

![2 6-2_ModifierStack_Seeds](https://user-images.githubusercontent.com/18192380/236580650-e2f7128c-0998-4720-bd0c-5e31cfaf9207.png)

Image 2.6-2: Adjustable seeds in the modifier stack.

Change these in the modifier stack for immediate results or keyframe them and check through several iterations by using the timeline. 

- XY-Scaling

The XY-scalings for small, medium and large limb-parts can also be adjusted in the GeometryNodes for that style.
Modify the vector-values, which act as min-size/max-size for both EndLimbs and MidLimbs. X, Y and Z are affecting the XY-Scale of small, medium and large limbs, respectively.

![2 6-3_XYSCaling](https://user-images.githubusercontent.com/18192380/236580663-99ee4801-4320-48e6-9d9a-332d2ba9116c.png)

Image 2.6-3: Adjustable scaling in GeometryNodes.

The 'XY-Scale'-Value below these four vectors can be changed to make all limbs bigger/smaller at once. Some of the styles already have a slight number adjustments, play around with these to see the effect.

- LimbLength

The length at which a small limb turns into a medium or large limb is adjustable.

![2 6-4_LimbLength](https://user-images.githubusercontent.com/18192380/236580673-e8d95e10-1e92-45b5-82ef-df3fe549b97e.png)

Image 2.6-4: The LimbLength-values in GeometryNodes.

Note that scaling the control structure and applying the scaling will cause the geometry instances to update, which will change the overall generated mesh.

- LimbEnds in LimbMids

There is an option to generate LimbEnds into every LimbMid. This was at first unintended, but also resulted into more randomness, especially when using organic shapes.
This effect can be re-enabled by using the Boolean 'Generate LimbEnds in LimbMids' inside the GeometryNodes. 

![2 6-5_GenerateLimbEndsInLimbMids](https://user-images.githubusercontent.com/18192380/236580681-8d798855-45a5-4871-b6c5-b90465916636.png)

Image 2.6-5: The marked Bool-option in GeometryNodes.

- Polishing the mesh

As the geometry nodes can create clipping meshes, the remesh modifier might help to reduce complexity. 
Applying a remesh modifier after the geometry nodes modifier on the control structure will create a smoother surface. Apply in order when you want to generate UVs or bake the texture.
By my experience, voxel-based remesh tends to have smoothest results - you can also try to use Sharp with an level of about 8 and uncheck the 'remove disconnected' - this can create more worn-down results, especially at parts where the LimbParts overlap. 
When using a remesh, your mesh will lose the material data from the instances. When using only a single material, using an new GeoNodes-Setup with only a 'Set Material'-Node might fix this.

Save your progress regularily. 

# 2.7 ADVANCED WORKFLOW - Geometry Nodes

With the 1.26 version, the geometry nodes can also use named attributes for rotation. 
Other GeoNodes, that are in the modifier stack above, can set the named attribute 'LimbRot', which will then automatically overwrite the rotation target for the Limb Gen-GeoNodes.
In the main file you can find a simple example. This can be utilized for more complex creations.

# 3 CONTENT

This scene currently contains five different style of limbs: 
Switch between those in Object Mode and using 'shift + 1 to 6' to easily preview these. 

![3 1-1_DifferentStyles](https://user-images.githubusercontent.com/18192380/236580695-374ef105-b828-490d-90a6-c5cae6c962d1.png)

Image 3.1-1: Different Styles Showcase, as shown in order below

The current official styles are:

- Mecha: technical, blocky parts.

![3 1-2_Mecha](https://user-images.githubusercontent.com/18192380/236580701-1c5d9790-a99a-4019-b152-2c4185ac8ba7.png)

Image 3.1-2: Mecha Style Thumbnail

- Organic: a selection of shell-like parts.

![3 1-3_Organic](https://user-images.githubusercontent.com/18192380/236580726-6644324a-4405-474c-a959-bd77e5898a8b.png)

Image 3.1-3: Organic Style Thumbnail

- CyberneticSaw: edgy technical parts.

![3 1-4_CyberneticSaw](https://user-images.githubusercontent.com/18192380/236580732-ac521375-3214-4e61-87b8-7a1fa4cffb24.png)

Image 3.1-4: CyberneticSaw Style Thumbnail

- StylizedRoot: lowpoly root-like structures.

![3 1-5_StylizedRoot](https://user-images.githubusercontent.com/18192380/236580743-d407def5-e485-4eb5-9803-491a07671939.png)

Image 3.1-5: StylizedRoot Style Thumbnail

- Prosthesis: an experimental, very simple style for limbs. Might work better when not overlayered.

![3 1-6_Prosthesis](https://user-images.githubusercontent.com/18192380/236580748-20be9443-8d29-410c-9ee2-c635c99afc93.png)

Image 3.1-6: Prosthesis Style Thumbnail

- Generic: an style to adapt to your liking or to use as a basis. Not shown in the above setup.

![3 1-7_Generic](https://user-images.githubusercontent.com/18192380/236580756-7e40c3a3-2b5a-4723-9378-b4522766132b.png)

Image 3.1-7: Generic Style Thumbnail

Each can be used by setting the respective GeometryNodes modifier to one of the 'Limb Gen (STYLE)'.

These control structure meshes are found in every style collection. These are duplicates and only differ in which geometry node setup is used. These meshes do not have a set material themselves.

The instanced LimbPart-meshes have a simple ambient occlusion based material applied to them. These require rendering in cycles for correct display.
Consider using a 'set material'-node in the GeometryNodes after the 'Generic Creature Limb Generator'-node, if you quickly want to swap out the material for the whole generated mesh.

# 5 CURRENT ISSUES AND LIMITATIONS

Many issues have been resolved since the 1.08 version. 

- LIVE EDITING PERFORMANCE

Based on the power of your rig, you might want to stay in simple shaded for editing.
The base scene contains a deactivated remesh.
Editing the meshes of LimbMid and LimbEnd can require more perfomance when remesh is activated. If you only want to change the limbs, perhaps consider disconnecting the final geometry nodes to skip recalculations until your changes to the Limb-Meshes are finished.

- RIGGING

This tool only creates instanced geometry. Applying the modifiers of the control structure is recommended. You could use the skin modifier on the control structure with reduced vertices to create a base rig. 

# 6 SUGGESTIONS AND FUTURE
You can contact me in my discord server (https://discord.gg/BJr6rT3Cer) for suggestions, troubleshooting and/or creations! 
