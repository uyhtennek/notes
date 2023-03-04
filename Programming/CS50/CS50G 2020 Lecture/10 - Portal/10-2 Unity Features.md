[MODELING IN UNITY?! - ProBuilder Tutorial - YouTube](https://www.youtube.com/watch?v=PUSOg5YEflM)
[THIS TOOL IS AWESOME - ProGrids Tutorial - YouTube](https://www.youtube.com/watch?v=UtNvtIrJcNc)
___

# ProBuilder

when we apply texture to the models in ProBuilder,
It can applied on a face, instead of the whole model

It provides tools to create special kinds of geometry very quickly and efficiently

We can find it on the Asset Store
* It comes from Unity Technologies
	* and anything comes from Unity Technologies is sort of a free supplement to Unity
* After we imported it, we can see from Tools and find ProBuilder and ProGrids
	* the two are seperated, not the same thing

To start using it, we can click on Tools > ProBuilder > ProBuilder Window
* we can create new shapes, or shape templates
	* each types of shape has a bunch of fields that we can manipulate and affect the polygons
	* eg. Stairs
* once we're satisfied, we can click Build Stair, then it's done
	* now we have a stair that we can put any where in our level

After we built a shape
* we can also change its shape, by manipulating its vertices, edges, and faces
* much like how the 3D modelling software works, except it surely has less functionality
	* eg. we can't do any rigging in ProBuilder

And we can export the models as well
* as FBX or OBJ
	* so maybe we're happy with the level design, then we want to open up Blender or Maya or whatever 3D software to make the scene prettier
* also we can export it as Asset, so we can make game object Prefab this way
___

# ProGrids

it's a add-on, which will lock everything in our scene, to a specific grid
* so things move on a grid

Why it's an advantage?
* basically it ensures everything is very clean
	* sometimes we want the things to be aligned or arranged in some specific way
		* eg. want to even line up a few cubes
	* handy to do with ProGrids
		* otherwise we may find ourselves setting the cubes' Transform XYZ values in the Inspector manually, which is a pain
* somehow can make drawing textures for the level easier
___

# Gray boxing
*gray boxing* means making levels and testing them for playability


## Interior Level

we can invert normals for meshes
* most 3D software has this feature
* Open ProBuilder window > click Flip Normals

> [!Normal]
> Every 3D polygon, every 3D surface has an normal. In which direction that it is facing, so the face that the surface is facing, gets rendered. But if we look behind it, behind the surface, the opposite direction of the surface's normal, it's invisible.
> 
> That's how we can, in some games, clip through other character's head and look at the world inside of the head. Often times can see the character's eyes and mouth as well. Because we're looking behind of the head's surface normal.

So if we flip all the normals of something that is convex,
we get an interior scene
* if we look into it from the outside, we can see into it

Normally creating an interior scene can be a pain in the butt
* now we can just do it easily inside Unity Editor

> [!tip]
> Collision on a plane can depend on the normal as well. Sometimes it only trigger the collsion when we hit it in the front, but hitting it in the back doesn't trigger a collision, sometimes it does, depending on how the engine implemented it.
> 
> So often times we can see walls of a house model is actually two planes. One facing in and one facing out. So things can hit it in both sides.


## Material Editor

we can assign to it some materials, then select some faces, then click Apply
* the material will be applied to only the selected faces
* rather than applied to the entire mesh

There is also an UV editor that ProBuilder provides to us
___

# Shader Graph
another (relatively) new feature coming since 2018
* Unreal has it for a long time already though

Rather than having to write shaders, in Shader Lab, which is Unity's shader language
* can be quite an experience....

We can actually create shader with a node based programming environment
* we choose these preset nodes and manipulate them to influence the shader's behavior
* it allows us to see the shader, how the nodes affecting the material, every step of the way

So shader is just a series of transformations
* going from left to right in the sense of shader graph
* we can see how each node accumulating to produce the final effect

We used to write code to implement shader
* which is quite non-trivial
* now we don't write code at all, but generate code, using shader graph

To use it, go to Window > Package Manager
* then find Shadergraph
___
