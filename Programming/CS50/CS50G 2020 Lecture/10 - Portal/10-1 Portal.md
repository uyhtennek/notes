# Portal

* very iconic game
* we have a portal gun, that shoots elliptical portals
	* if we look through one, we can see what is coming out of the other one
	* if we walk between them, we get teleported to the other side
* allows some cool tricks and solve puzzles

as a disclaimer, there is a bug in our version of Portal
* If we shoot 2 portals on 2 different walls, one of the portal would be upside down
	* if the 2 portals are on the same wall, it's fine though
* because the upside down portal is in a state called *gimbal lock*
	* for some reasons, Unity's internal representation of a rotation can get messed up, when we're manipulating an object *euler angles*
		* euler angles allow object to rotate on axes independently
	* mess up in a way that 2 axes are locked together
		* meaning the 2 axes will both rotate each other

Also, the portals are actually placed some where in the scene even though we haven't shoot any portal yet
___

# Holding a Gun

How are we keeping the gun affixed to where the camera is looking?
Just make the gun a child of the camera.
* it has an effect of setting the gun's transform to be the same as the camera's, but with an offset
* the effect also takes place in rotation, and scale
	* so when we make any rotations to the camera, the gun stays exactly aligned with the camera
* the effect is recursive. it affects all the children of all the children of all the children .....
___

# Raycasting
It's a nice feature that Unity gives us, for free.

* It's part of the Physics namespace in Unity, part of the scripting API
* It allows us to effectively look from whatever transform or whatever point we give it as the source, at a direction, which is a vector
	* in our case we want it to look at `transform.forward`
	* which means straight in the Z direciton
		* if we're doing it on a camera, it's effectively what ever direction we're looking at
		* coming out of the center of screen or camera

* we are actually casting a ray from the tip of the gun though
* to its `transform.forward`

Unity has a function called `Debug.DrawRay()`
```c#
public class DebugRay : MonoBehaviour {
	void Update () {
		Debug.DrawRay(transform.position, transform.TransformDirection(Vector3.forward) * 1000, Color.red)
	}
}
```

* the source point is `transform.position`
* the second argument is the direction, and we times it by 1,000 so the ray travels 1,000 units
* and lastly give the ray a red color

The ray here is only in the Editor view though, it doesn't apply in the actual game
* It's a `Debug` function after all
* so the ray is rendered in Scene view but not Game view

Primarily, raycast is how we check whether something is blocking something else
* maybe in a game like GTA, a car is moving and it should check whether or not its front or back has another car
	* we can just cast a ray and see if there's any geometry there

Here is the place we actually do the ray cast
```c#
// file: PortalGun.cs
void FirePortal(string type) {
	RaycastHit hit;
	if (Physics.Raycast(gunTip.transform.position, transform.TransformDirection(Vector3.forward), out hit, Mathf.Infinity)) {
		// there is a hit, and
		// all information about the collision is in `hit` from this point forward
		portalSound.Play();
		
		GameObject portal = type == "orange" ? orangePortal : bluePortal;
		// flip the portal so that it faces the same side as the wall
		portal.transform.SetPositionAndRotation(hit.point, Quaternion.FromToRotation(...))
	} else {
		errorSound.Play()
	}
}
```

`RaycastHit hit;`
* `Physics.Raycast()` actually returns a struct object
* we store it in this `hit` variable, and it tells all the information about the hit
	* eg. what the *normal* was on the surface that it collided with
		* normal means in which direction the surface is facing, or projecting out
* recall [[CS50x 3-3 Data Structure#Custom Data Structure|a struct, is a collection of variables]]
	* like in C or C++

`out hit`
* `out` is the C# way to return multiple values from a function
	* normally when we pass in a variable to a function call, it doesn't get manipulated
	* but we can declare an argument as `out`, to allow the function to change its data inside
* in this case it returns a `RayCastHit` struct object, and we stored that in `hit`

`Mathf.Infinity`
* as it said, it's infinity
* so the ray just cast to infinity
	* Unity obviously doesn't check infinitely
	* it's somehow optimized
* often times we don't want to check for like 2 units, 5 units, or a thousand units, or any specific distance, but just to check if it collides with something, intersects with any sort of mesh


> [!btw]
> The name 'ray casting' is also used for a method in rendering. Used in old school games like Wolfenstein.
> 
> We generate a ray from every pixel of the screen, or actually it was mostly just every line of the screen, up and down. Then its level is composed with very simple level geometry. Then based on how far the ray goes before colliding with a wall, it looks the corresponding pixels from a specific texture and draw that onto the screen.
> 
> So although the game is 2D, its world looks 3D, but we couldn't move up and down, because it was always generating all the rays completely forward.

___

# VR in Unity

Firstly we need a PC for that, version 2018 of Unity doesn't support VR on Mac

Then go to Edit > Project Settings > Player
* then find the XR Settings
	* Unity has deamed all of its VR, AR stuffs as XR
	* check Virtual Reality Supported

We need to plug in our VR device
* it'll just work with the camera, right out of the gate
* we may need to install drivers on our computer so it knows we have an Oculus plugged in

Unity (2018) supports HoloLens, Oculus Rift, Vive
___

# Render Texture

A render texture, is just a texture in Unity
* so it's an asset, an Unity asset that we can create

The difference between a render texture and a regular texture that we imported from like Photoshop or Gimp
* is that a render texture can be rendered to
* typically, it's used for things like cameras, being rendered to it
* but actually we can render anything to it
	* so we can create procedural textures this way as well

Think of it as a screen
* what the screen is showing is essentially the viewport of the other portal
	* so that we know what it will look like once we walk out of the portal

To create a render texture, just do Create > Render Texture
* now what happen if we set the texture size pretty small, maybe like 200 pixels by 200 pixels, but our game is like 1920 x 1080?
	* it'd look pretty pixelated, pretty nasty
* the solution to this would be, to dynamically figure out at runtime, what's the resolution of the game
	* so no matter the game is 4K or 720p, it just creates a render texture with the same size
	* then even if we're right up close to the render texture, it fills up our whole screen, but it wouldn't be pixelated at all
* In our game though, we don't need that to be that robust, so we just set it to 1024 x 1024, figuring that it would be good enough for demonstration purposes
	* btw, the default size of a new render texture is 256 x 256, pretty small

> [!tip]
> Generally, a high resolution is not an issue. Game textures are generally very high resolution. 4K textures are often used, even if the game is just 1080p.
> 
> A game engine will probably optimize it, to down sample the texture so that it's actually like a 1080p texture. So we're not trying to calculate and draw more than we need to.

Now we have our render textures ready, but by default they are not gonna be mapped to anything
* they're just empty render textures now
	* later we will be rendering to these when a portal is opened
	* like blank screens when the TV hasn't been turned on yet
* in order to render to them, we go to the camera that we want to render to the texture
	* just to clarify, we're taking the render texture, and we're rendering a camera view onto it
	* the camera is placed behind the portal, looking out from it
		* and we want to take that view, and render that to the other portal's face, the other portal's render texture
* Every camera has a Target Texture field, that specifies the texture to render the view on
* also notice only the main camera's view get rendered on our screen

Lastly we're setting the render texture to be the material of a mesh, a plane

One last thing, is that in our game the portals are not perspective correct
* when we look into a portal from an angle, we still can only see the front of the other portal
	* instead of the side of the other portal, like the Portal game does
* to fix that we gonna need the cameras to track the player position relative to the portal, and move the cameras behind the portals accordingly, with the player
___

# Texture Masking

Now notice, a plane is just a rectangle
* we can't really create a circle shaped plane
* that doesn't really exist
	* even if it does, it wouldn't be efficient
	* the triangles and polygons on that circle shaped plane would be messed up
		* depending on how smooth we want the circle is, the mesh would get very high res

And a render texture, is just a texture applied to a material
* it has nothing to do with the shape of the mesh

So the optimal way to create an ellipse plane, is to give the plane a texture, which we designate
* some pixels as being pixels that we want to read
* and pixels that we don't want to read
* therefore it produces a final image that put onto a geometry

So we have a texture, it's just an image with a simple oval
* which is the shape that we want the portals to look like
* and there are white and black pixels, and the white pixels are the part that we want to render
	* we're effectively using the pixels' alpha

Then we also have a masking shader
* provided by Unity
* it has a few characteristic including turning lighting off
	* so shadows and lights don't cast on our portals
	* we don't want the portal to look like a sort of a glass door
* it accepts a Culling Mask
	* then it combines the mask and the base texture
	* and then cancel out any of the pixels that are black on the mask
		* it effectively set the base texture's alpha channel as the mask texture's
		* so we can make some pixels gray to make it half transparent as well

> [!tip]
> Often times we need to take away details from a mesh or material, or we want to some really cool effects, it's often just a lot easier to create a mask for it. Then use the right shader that's meant to have that mask.

**Is cylinder easier?**
maybe we can create a cylinder, and make it really really thin, then just do the render texture thing
* it still has depth, which is not very ideal, because it doesn't make sense in our use case
* when we apply texture all it, it would be applied in all side
	* so for cylinder it would be 3 sides, plus the UV mapping can get weird
	* plane on the other hand, has just 1 side
* dealing with UV mapping is not as easy as making a texture mask
___

# Teleporting

just make a collider for each portal
* in our case it's a mesh collider
* and make it a trigger

then in `OnTriggerEnter()`
* teleport the other collider to the other portal

```c#
// file: Portal.cs
public GameObject linkedPortal;

// as soon as we touch a portal, and trigger the teleport, we disable teleporting
// so the other portal doesn't teleport us immediately back to the entered portal
private bool portalActive = true;

void OnTriggerEnter(Collider other) {
	if (portalActive) {
		linkedPortal.GetComponent<Portal>().Toggle();
		Toggle();
		
		// do the teleport
		...
		
		// basically re-init FPSController
		other.GetComponent<FirstPersonController>().MouseReset();
	}
}

void OnTriggerExit(Collider other) {
	Toggle();
}

public void Toggle() {
	portalActive = !portalActive;
}
```

keep in mind that our portal system is a very simple one
* nothing as fancy as the Valve's one

One thing to be careful though, is that performaning rotation on FPSController is kinda hard
* since its rotation is based on movement of the mouse
* if we change its rotation in code but not via mouse, it'll set immediately back to its prior rotation

For a better portal system, see
[Smooth PORTALS in Unity - YouTube](https://www.youtube.com/watch?v=cuQao3hEKfs)
___
