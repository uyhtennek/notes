# Dreadhalls

* it was a VR game, on Oculus from Samsung
* we're in a dark and eerie 3D maze
	* we can go around to get collectibles
	* and encounter monsters and stuffs
* first person camera
	* the camera is effectively our eyes
	* as if we're walking around in the maze ourselves

> [!btw]
> Unity has some standard assets, for quite a number of things.
> 
> We imported a Character package from it, and used a non-physics based first person controller.
> Also there is a Prototype package which offers some 3D items.

___

# Texturing

If we add a Cube to a scene
* we can see that it gets a Default-Material
* which has an Albedo fields
	* basically just means what the material's surface color look like
	* it has a much more technical definition to it which has something to do with the way that light interacts with surfaces
* there are other things like Metallic, Normal Map, Height Map, etc.
* we can also make a material emits light

* If we put a very small texture on a very large cube, how it'd look like?
* it's going to look kind of like an N64 cube
	* because it will interpolate the texture pixels, the *texels*, to stretch the texture
* the texture will look filtered, kinda like if we blow up a very low resolution YouTube video
* what we can do is to apply Tiling
	* to tile the applied texture several times to get the desired look that we want
	* eg. maybe we have a very small character looking at a very large wall which has many bricks

Preview for materials in Unity by default has a Sphere look
Textures though are just 2D images

To apply a material to a game object, just drag it to the game object in the Hierarchy or Scene
* we can apply a texture to a material by assigning the texture as the material's Albedo


## Texture Mapping

it's actually a fairly wide field and fairly complicated topic when it comes to texture
* but ultimately it's the art of applying a 2D texture to a 3D model

basically we take all of the polygons that comprise the model, and then laid them out flat
* lay them out flat as if on a table, and we'd do the mapping on the table
* so the texture's virtual coordinates maps to the 3D model
* that's what UV mapping is
	* usually this procedure is done in a 3D modeling software

Unity though, has it own mapping algorithm
* for applying material to a model
* usually works well for its primitive objects

* but if we have let's say a table object or a character object, and then just apply to it a basic material and a texture, it's not going to look good
* therefore our 3D modelling software would export a material, along with the model
	* so that when we bring it to Unity, it can properly apply the texture
* for complicated model, it has to be UV mapped, in a smart way or a correct way
	* Unity's UV map is not very smart

as a proof, if we apply a material with a texture to a cube, and then scale its y-axis a bit
* we can actually see the texture is being compressed or distorted
* if we don't want this flattening happening, we actually need to do UV mapping in our 3D modelling software
	* we need to unwrap the model and then apply a texture to each separate polygon in a way that it looks convincing

> [!conclusion]
> If you are creating your own 3D assets, or if you're importing 3D assets, and if you want to use textures, you will need to become familiar with UV wrapping, UV unwrapping, UV mapping in whatever software that you like.


## Reading

[Catlike Coding](https://catlikecoding.com/)
* totally free
* very in-depth, far beyond than just the surface level
* btw, advanced materials usually involve several layers of map

> [!info]
> Unity uses HLSL, *High Level Shading Language*. Microsoft originally came up with it. And it's very similar to GLSL, *Open GL Shading Language*.
> 
> So shaders are effectively little programs that run on our graphics card, and they can tell the scene how to process lighting for the objects that are within it. And material in Unity has a shader associated with it.

We can write our own shaders
* so unlimited possibility


## Material and Shader

Material is effectively, a shader
* but with material we can also specify how its surface should interact with other things
* eg. if we have an ice level, a material can be how glossy is the ice, how it looks, but also can be how slippery it is when I walk over it
	* have physics

As to begin, you're just gonna worry about Albedo, Metalic, Emission, Tiling and Offset
___

# Lighting

Material is one part of the equation. It defines how things should look when light hits them.
But we also need light itself in our scene, to illuminate things.
* again, visit [Catlike Coding](https://catlikecoding.com/)


## Point Light

a light source that shoots out in all direction
* emits light in all directions
* but within a confined area, at a specific intensity
	* the closer it gets to an object, the more the object gets illuminated

perfect for things like lamps
* like a street light
* or a fire going on in a house
* often used on pickups too


## Spot Light

emits light in a specific diretion
* we can apply *cookie* to them
	* It is just like what the Batman light does, with the Bat signal
	* It allows us to apply a texture to a light, and therefore cast shadows, specific shadows
		* white means full light; black means full shadow;
		* and we can make it anywhere between white and black
	* so it's the same thing as taking a real spotlight and putting an object onto it, to produce a manual shadow


## Directional Light

Casts light in a single direction, but throughout the entire scene
* as if it's the sun
* it can illuminate globally the entire scene, but all light gets cast from one direction
* if we want to produce the appearance of daylight, just a single direction light is okay

so its position doesn't really matter
only its rotation does


## Area Light

emits light from a specifically-designated rectangle, in one direction
* less used
* we can define a large area, and have it to emit light
	* eg. a wall strip on the wall to emit light specifically to the left

* they are computationally expensive
* so we can only use them when we bake the lighting


## Baked Lighting

instead of real-time lighting, calculating things dynamically
The light gets calculated, one time, and saved.
* almost like the light is freezed onto all of the objects in the scene

the upside is - it's less computationally intensive
the downside is - no shadow. we can't cast shadows, other objects can't cast shadows on us.
* because the scene has been pre-baked
	* we're just recoloring the world, but not doing any lighting calculation

This is how lighting worked in, like the N64 era though
* for certain situations, it's how it still works now
* if we know nothing is going to cast a shadow on something, we can make really nice looking lighting for a scene, without needing to do it in real time


## Reflection Probe
and also Light Probe Group

those are more complicated, but from them
we can get pseudo-real-time lighting and reflection with baked lighting and reflection

> [!tip]
> Remember that we can change a light's color. Important to creating pretty scenary.

___

# Normal Mapping
as known as Bump Mapping

What a bump map allows us to do, is to take a flat wall or flat surface, and then simulate an actual three-dimensional contour, three-dimensional bumps
* without needing to create the actual geometry

There are different tools to create bump mapping textures
What a bump is effectively, just the encoding of what are called *surface normals*
* just a vector going from outside of the polygon at each texels
* they tell the lighting system in Unity, to pretend as if there's actually geometry pointed out in that direction
	* but really the surface is just flat

As a result, a flat wall can look as if it actualy has cracks and bumps in it
* we also can control how strong the bump map will affect the lighting rendering

Often bump map looks quite colorful or pretty
* because its RGB is the XYZ permutations for the surface normals
* therefore sometimes we can tell how bumpy or contoured the object's surface is, just by looking at the bump map
	* the texture itself sometimes can give us a sense of the shape of the object or surface
	* since it shows where is the highlight and shadow
___
