# BLENDER-Creature-Limb-Drawer

This asset is built around a system of GeoNodes.
The basic idea is: "draw a stick figure with vertices, get a creature".
The current iteration is built with blender 4.2.

![1 Intro](https://github.com/user-attachments/assets/38bb05d1-f651-4589-8473-51ad13002fcb)

![2 Main Header](https://github.com/user-attachments/assets/e9e6ab7b-216a-46f0-bbb5-5bcb07d04b01)

The instance spawn behaviour per edge can be costumized.
Feel free to edit the GeoNodes to your liking.
Besides CLD, this asset also contains a 'Instances on Edge'-Node, which is a heavily reduced version of the CLD2-node graph.

# 1 CLD-EXPLANATION

This tool creates randomized instances along edges. 
Unlike the included 'Instances on Edges'-Node, the rotation and length of the edge is taken into account when spawning instances.
The orientation ('which vert of the edge the instance will look towards') can be customized as well.

Setting one (or more) starting points (via a named vertex group) will make all connected edge-instances orient away from the starting point, allowing you to set a direction by marking single vertices. This must be done for each mesh island, or they will retain a semi-random orientation, except for the end-parts.

Extruding multiple edges along each other can increase the volume of the spawned limbs.

![3 Hand Example](https://github.com/user-attachments/assets/51bbe561-52da-4476-8f7b-7b1bdb1f9c0a)

# 2 CLD-USAGE

Add the CLD 2.0 GeoNode modifier to your mesh. Ideally, you should not start with a high-resolution sculpt.
Set the collection references in the modifier. You can use the included collections of this asset or create your own.
Ideally, the meshes in the collections should have dimensions about X=0.2, Y=1.0, Z=0.2. You can use the included 'Debug Limb' as a base.

![4 Limb Dimensions](https://github.com/user-attachments/assets/38dee20d-334c-46d2-ae77-4ef44c49d6f7)

Extrude edges or make edges between vertices. CLD 2.0 is set up, so that you will get either 'limbmids' or 'limbends', depending on the amount of connected endpoints for edges.
'Limbends' are determined as edges, in which at least one vertice is not connected to another edge. Useful for fingers/spikes.
'Limbmids' are edges, where both verts have at least two edges connected. Anything that is not an endpoint. If an edge endpoint is also marked as a StartingPos, it will count not count as a starting point for simple generator.

![5 Different Shapes](https://github.com/user-attachments/assets/a2a5e440-0f9d-457c-a61f-ebc4d5e86052)

![6 LimbEnds LimbMids](https://github.com/user-attachments/assets/9f5c1846-0b6c-42bc-bc01-3defe2ca8efd)

Modify the GeoNodes to your liking. 
The simple version is built around three edge-lengths, resulting in 3 collections for 'limbmids' and 3 collections for 'limbends'. 
Per edge, depending on length and if it is a 'limbend' or 'limbmid', exactly one instance is spawned from the associated collection pools. 

# 2-1 Limb Orientation

Aka yaw+pitch / ('to which vertice of the edge will this instance point towards')

Orientation is determined arbitrary, unless at least one of the connected vertices of the mesh island is set as a specific vertex group ('CLD Starting Pos' by default).
Once set, all connected edge-instances will rotate towards the vertex that is furthest away from the starting point.

By using the 'CLD Starting Pos'-vertex group, you can set to which end the rest of the connected edges will rotate their instances to.
Per mesh island, setting one (or more) vertices as the starting points will make all other limbs of that island spread out from these verts.

Most examples in the scene use a vertex group to orient the instances.

![7 Limb Orientation](https://github.com/user-attachments/assets/5724805b-3658-4bc3-a07f-33c744b17bf8)

# 2-2 Limb Rotation

Aka roll / ('how is this instance rotated around the edge')

Limbs spawned will have an arbitrary roll value.
If you want a custom rotation, you can use the provided extra modifers before using the main CLD-Modifier. These extra-modifiers can be used (and stacked) to rotate the instances towards proxy objects/geometry/at random.

Many examples in the scene use modifiers to manipulate the Limb Rotation:
- Rotate towards target object origin
- Rotate towards target mesh surface
- Rotate random

![8 Rotation By Object](https://github.com/user-attachments/assets/0dec3c6f-d70b-4508-b89e-6ae22f27f981)

# 2-3 Limb Styles

You can set the style of a limb by setting its start/end vertice to a calculated Value.
Styles are based on a vertex group. These are meant to be set manually, but can also be created/manipulated by other GeoNodes. See examples for this.
The GeoNodes need to be set up to support multiple styles, which will also dictate the values to set the vertex group.

By using the 'CLD Style'-vertex group, you can switch between sets of collections. There is some slight amount of math involved:
To use two style collections, the following values for 'CLD Style'-vertex between...
... 0 and 0.5 will result in Collection 1
... 0.5 to 1.0 will result in Collection 2.
For three, these will range between 0-0.333-0.666. Basically every 1 divided by your max integrated styles.

The amount of styles must be set up in the GeoNodes. Look for teal nodes, if they have a number related to style, change it accordingly. 
The geonodes were not designed for programmers; style related numbers start at 1, not 0.

The used main 'CLD2 - Limb Generator' must contain multiple set up styles to support this feature.

A few examples in the scene use style to switch between sets of collections.
- Styles can also be randomized via modifiers. See the one example in the scene. But mostly they are meant to be set manually.

![9 Set LimbStyle](https://github.com/user-attachments/assets/c602df05-f5a6-4f86-b18a-e4796f02b321)

# 2-4 Basic GeoNode options

The main modifier are the 'CLD2 - Limb Generators'

![10 Available Options](https://github.com/user-attachments/assets/62c130f7-f132-4fe1-8ee3-15a4737eabb6)

Seed - Allows you to randomize the instance of each collection by a single value.

Debug - Allows to render the limbs with a lowpoly version, showing the direction and orientation. Useful if performance is a concern.

Realize Instances - Is required for further mesh transformations via modifiers/geonodes (like a mirror modifier below CLD). If you want to turn the CLD-instances into a mesh by applying it, this must be true, or you will not get resulting geometry when applying the modifier.
Unchecked: improves performance but modifiers in the stack below CLD might not work.

Lots of Collections - These were added to quickly swap around collections to see the changes. The amount of collections / naming of these collections differs per example.
Other 'CLD2 - Limb Generators' have tons of collections due to supporting basically having 'multiple CLD - Limb Generators' included the same time.

# 2-5 Asset browser

Use the asset browser to speed up your workflow and pre-create limbs that you want to use more often. 

![11 Asset Browser Add](https://github.com/user-attachments/assets/daf3ea80-4380-49b8-804b-85204c109883)

Unlike previous versions, this requires more manual work before this is fully set up, depending on your choices.

The natural workflow now is:
Drag one of the example modifiers, like the 'CLD2 - Limb Generator - Simple' onto the edge-mesh. 
Then add one of the style collections of your choice via the asset browser to your scene.
You will need to set up the collections in the modifier manually. This allows you to mix-and-match more quickly, but comes at the cost of initial set up time.

If you want to create your own preset, duplicate a 'CLD2 - Limb Generator' (most likely the 'Simple' or 'Basic' version) in the GeoNode Editor, then right click 'Mark as Asset'.
Remove the Collection Group Inputs if you so choose, and place the Collections in the GeoNodes manually. When using your custom-version, all corresponding collections will be added to the scene as well!

If you want to use a similar thumbnail like the ones in the plugin, you can use the collection 'Asset Browser Image Render' to render these. Rendering in the base scene is only enabled for the Asset Browser Image-Collection.
Note that switching between different LimbGens will not work as expected due to internal naming schema. For safe result, remove the selected GeoNode first, then switch to the one you want to preview/render.

![12 GeoNode Switch](https://github.com/user-attachments/assets/3c3969f6-b74f-43d2-9395-43f7940ee4a7)

# 2-6 Example Content

Besides technical examples, all previous collections of limbs are present (as collections only).

Collections contain Limbs for:
Organic: a selection of shell-like parts.

![LimbGen - Organic](https://github.com/user-attachments/assets/a7c9b1fc-f09b-4e15-9e8d-fa40bf195d3e)

Mecha: technical, blocky parts.

![LimbGen - Mecha](https://github.com/user-attachments/assets/cb4f3c4c-d04f-47cd-bbe5-6d974130adc5)

Cybernetic: edgy technical parts.

![LimbGen - Cybernetic](https://github.com/user-attachments/assets/208c813b-4d26-4682-90ef-db04b831e009)

Prosthesis: experimental, very simple style for limbs. Might work better when not overlayered.

![LimbGen - Prosthesis](https://github.com/user-attachments/assets/66251cee-3f78-4a94-be71-af968ce4bac9)

Root: lowpoly root-like structures.

![LimbGen - Root](https://github.com/user-attachments/assets/63d4a889-c612-4424-a95e-e8c756d12673)

Generic: an style to adapt to your liking or to use as a basis. Not shown in the above setup.

![LimbGen - Gen](https://github.com/user-attachments/assets/f564871a-8a20-46cf-8922-1fce5929f2c0)

SCREENSHOTS: Available limb generator modifiers

These used to be separate modifiers, marked as assets. They are still inside the file. If you want to reuse these modifiers, you need to mark them as assets via right click on the GeometryNode-modifier. 

# 2-7 Shader

This plugin does not focus on shaders, as such, these are held very simple. The geonodes save their limblength and use it to change color for some of the shader parts. 
Realize instance must be enabled for this to work.

![13 Shader Values](https://github.com/user-attachments/assets/57b5d203-0a3e-46f2-b9b8-4f2dfc10abc5)

Important changes since last version:

- Shortened readme.
- Reworked limb creation from scratch, resulting in much more stable results.
	- Endpoint detection improved.
	- Order of limbs can be forced via vertex group.
	- Rotation of limbs can be changed via modifiers.
	- Allows you to switch between different collection sets via vertex group.
- GeoNodes and subnodes are named more precisely.
- Used subnode for storing the attribute names in GeoNodes.
- Removed the 'create limbends in limbmids'-feature. If you want a similar effect, you need to modify the geometry nodes. 
- Reworked different styles from previous version into the new system.
- Shaders for old content have been slightly changed. 
- Added a 'Instances from Edges'-node, which is a feature-reduced version of this system as a separate file. It offers the options of the 'Instances from Points'-node (on which it is based) + allows you to chose where to spawn along the edge. If you don't need the complexity of CLD, use this instead.

Thanks for checking out my add-on - after a long time in development, CLD2 now contains all features I wanted to implement and can be controlled fully!
Feel free to look into the GeoNodes and adapt them to your needs.
For further questions on usage or how the GeoNodes work, ask me in my discord server: https://discord.gg/BJr6rT3Cer - also feel free to share your creations here :)
