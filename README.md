# BLENDER-Creature-Limb-Drawer

This asset is built around a system of GeoNodes.
The basic idea is: "draw a stick figure with vertices, get a creature".
The current iteration is built with blender 4.2.

GIF: 1 INTRO

Screenshot: 2 OLD MAIN HEADER IMAGE

The instance spawn behaviour per edge can be costumized.
Feel free to edit the GeoNodes to your liking.
Besides CLD, this asset also contains a 'Instances on Edge'-Node, which is a heavily reduced version of the CLD2-node graph.

# 1 CLD-EXPLANATION

This tool creates randomized instances along edges. 
Unlike the included 'Instances on Edges'-Node, the rotation and length of the edge is taken into account when spawning instances.
The orientation ('which vert of the edge the instance will look towards') can be customized as well.

Setting one (or more) starting points (via a named vertex group) will make all connected edge-instances orient away from the starting point, allowing you to set a direction by marking single vertices. This must be done for each mesh island, or they will retain a semi-random orientation, except for the end-parts.

Extruding multiple edges along each other can increase the volume of the spawned limbs.

SCREENSHOT: 3 Hand example

# 2 CLD-USAGE

Add the CLD 2.0 GeoNode modifier to your mesh. Ideally, you should not start with a high-resolution sculpt.
Set the collection references in the modifier. You can use the included collections of this asset or create your own.
Ideally, the meshes in the collections should have dimensions about X=0.2, Y=1.0, Z=0.2. You can use the included 'Debug Limb' as a base.

SCREENSHOT: 4 LIMB EXAMPLES + Dimensions.

Extrude edges or make edges between vertices. CLD 2.0 is set up, so that you will get either 'limbmids' or 'limbends', depending on the amount of connected endpoints for edges.
'Limbends' are determined as edges, in which at least one vertice is not connected to another edge. Useful for fingers/spikes.
'Limbmids' are edges, where both verts have at least two edges connected. Anything that is not an endpoint. If an edge endpoint is also marked as a StartingPos, it will count not count as a starting point for simple generator.

SCREENSHOT: 5 Different Shapes

SCREENSHOT: 6 LimbEnds LimbMids

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

GIF: 7 Setting the Starting point to affect orientation

# 2-2 Limb Rotation

Aka roll / ('how is this instance rotated around the edge')

Limbs spawned will have an arbitrary roll value.
If you want a custom rotation, you can use the provided extra modifers before using the main CLD-Modifier. These extra-modifiers can be used (and stacked) to rotate the instances towards proxy objects/geometry/at random.

Many examples in the scene use modifiers to manipulate the Limb Rotation:
- Rotate towards target object origin
- Rotate towards target mesh surface
- Rotate random

GIF: 8 Rotating limbs via a single point

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

GIF: 9 Setting limbstyles

# 2-4 Basic GeoNode options

The main modifier are the 'CLD2 - Limb Generators'

SCREENSHOT: 10 All available limb generators

Seed - Allows you to randomize the instance of each collection by a single value.

Debug - Allows to render the limbs with a lowpoly version, showing the direction and orientation. Useful if performance is a concern.

Realize Instances - Is required for further mesh transformations via modifiers/geonodes (like a mirror modifier below CLD). If you want to turn the CLD-instances into a mesh by applying it, this must be true, or you will not get resulting geometry when applying the modifier.
Unchecked: improves performance but modifiers in the stack below CLD might not work.

Lots of Collections - These were added to quickly swap around collections to see the changes. The amount of collections / naming of these collections differs per example.
Other 'CLD2 - Limb Generators' have tons of collections due to supporting basically having 'multiple CLD - Limb Generators' included the same time.

# 2-5 Asset browser

Use the asset browser to speed up your workflow and pre-create limbs that you want to use more often. 

SCREENSHOT: 11 Asset Browser Add

Unlike previous versions, this requires more manual work before this is fully set up, depending on your choices.

The natural workflow now is:
Drag one of the example modifiers, like the 'CLD2 - Limb Generator - Simple' onto the edge-mesh. 
Then add one of the style collections of your choice via the asset browser to your scene.
You will need to set up the collections in the modifier manually. This allows you to mix-and-match more quickly, but comes at the cost of initial set up time.

If you want to create your own preset, duplicate a 'CLD2 - Limb Generator' (most likely the 'Simple' or 'Basic' version) in the GeoNode Editor, then right click 'Mark as Asset'.
Remove the Collection Group Inputs if you so choose, and place the Collections in the GeoNodes manually. When using your custom-version, all corresponding collections will be added to the scene as well!

If you want to use a similar thumbnail like the ones in the plugin, you can use the collection 'Asset Browser Image Render' to render these. Rendering in the base scene is only enabled for the Asset Browser Image-Collection.
Note that switching between different LimbGens will not work as expected due to internal naming schema. For safe result, remove the selected GeoNode first, then switch to the one you want to preview/render.

GIF: 12 GeoNode Switch

# 2-6 Example Content

Besides technical examples, all previous collections of limbs are present (as collections only).

Collections contain Limbs for:
- Organic: a selection of shell-like parts.
- Mecha: technical, blocky parts.
- Cybernetic: edgy technical parts.
- Prosthesis: experimental, very simple style for limbs. Might work better when not overlayered.
- Root: lowpoly root-like structures.
- Generic: an style to adapt to your liking or to use as a basis. Not shown in the above setup.

SCREENSHOTS: Available limb generator modifiers

These used to be separate modifiers, marked as assets. They are still inside the file. If you want to reuse these modifiers, you need to mark them as assets via right click on the GeometryNode-modifier. 

# 2-7 Shader

This plugin does not focus on shaders, as such, these are held very simple. The geonodes save their limblength and use it to change color for some of the shader parts. 
Realize instance must be enabled for this to work.

GIF: 13 Shader Values

Important changes since last version:

- Shortened readme.
- Reworked limb creation from scratch, resulting in much more stable results.
	- Endpoint detection improved.
	- Order of limbs can be forced via vertex group.
	- Rotation of limbs can be changed via modifiers.
	- Allows you to switch between different collection sets via vertex group.
- GeoNodes and subnodes are named more precisely.
- Used subnode for storing the attribute names in GeoNodes
- Removed the 'create limbends in limbmids'-feature. If you want a similar effect, you need to modify the geometry nodes. 
- Reworked different styles from previous version into the new system.
- Shaders for old content has been slightly changed. 
- Added a 'Instances from Edges'-node, which is a feature-reduced version of this system as a separate file. It offers the options of the 'Instances from Points'-node (on which it is based) + allows you to chose where to spawn along the edge. If you don't need the complexity of CLD, use this instead.

Thanks for checking out my add-on - after a long time developing, CLD2 now contains all features I wanted to implement and can be controlled fully!
Feel free to look into the GeoNodes and adapt them to your needs.
For further questions on usage or how the GeoNodes work, ask me in my discord server: https://discord.gg/BJr6rT3Cer - also feel free to share your creations here :)