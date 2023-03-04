# Changing Scene

There are 2 places that we will load the Play scene
1. every time we grab the pick up coin
2. in the start screen, when we pressed Enter

```c#
// file: GrabPickups.cs
// attached to the FPSController
public class GrabPickups : MonoBehaviour {
	...
	void OnControllerColliderHit(ControllerColliderHit hit) {
		if (hit.gameObject.tag == "Pickup") {
			pickupSoundSource.Play();
			// we need to use the `UnityEngine.SceneManagement` namespace for this
			SceneManager.LoadScene("Play");
		}
	}
}
```

`void OnControllerColliderHit(ControllerColliderHit hit)`
* Character Controller has some function built-in that we can use
* this callback function is triggered when any collider hits our character controller collider
	* so it's actually calling every time the character collides with the walls or blocks as well
	* just there is no logic to run for that
* `hit` is the collider that we collided with

```c#
// file: LoadSceneOnInput.cs
void Update () {
	if (Input.GetAxis("Submit") == 1) {
		SceneManager.LoadScene("Play");
	}
}
```

`Input.GetAxis("Submit") == 1`
* Unity has a global input manager, `Input`
* There are different axes for input
	* it's weird but it's how Unity defines different methods of input
	* they are defined by keywords, like `"Submit"`
	* can see from Edit > Project Settings > Input
* Then we map those keywords to specific keys and input sources
	* `"Submit"` is either Enter or Return
	* and it could have other meanings if we're exporting this to Xbox or the web or mobile
* It explicitly expecting integer
	* if we just do `Input.GetAxis("Submit")`, it will throw an error if we try to use it like a Boolean
___

# Singleton

When we reload a scene, we destroy all the objects in the scene
* thus the audio sources attached on objects also get destroyed
* then when we loaded a scene, all the objects get re-instantiated
	* so is the audio sources
* resulting in the audio suddenly stops and then start over from the beginning
	* which is kinda odd, if it was the music

But we don't want that, we don't want the audio awkfully stop
* the solution is, there is a Unity function that allows us to carry over game object between scenes

But then there is another problem
If we don't destroy this object, then we load the same scene, because the scene will instantiate a new one, we eventually get 2 same objects
* we're going to have 2 audio sources of the same, playing at the same time
* what happen if we load the scene another time? we have 3 audio sources of the same.

To avoid this, we would use something called a *singleton*
* It's a class that can only be instantiated one time

```c#
// file: DontDestroy.cs
public static DontDestroy instance = null;

void Awake() {
	if (instance == null) {
		// so the very first DontDestroy instance, is the one will perserve
		instance = this;
		DontDestroyOnLoad(gameObject);
	} else if (instance != this) {
		// for the second, the third, the forth and so forth,
		// since `instance` is the first DontDestroy but not this one, destroy this one
		Destroy(gameObject);
	}
}
```

`void Awake()`
* it's almost like the `Start()` method
	* both are called when the game object or the component is instantiated
* except when we pause the object, and then we unpause it, then it will call `Awake()`

> [!tip]
> There will only ever be one singleton.
> 
> Singleton is a very basic, very common pattern in software engineering, for ensuring there only is one object of a given type present throughout the entire project.

___

# Fog

Some old games would have fog that is very dense, looks pretty unconvincing
* eg. Turok for the N64, Star Wars: Shadows of the Empire, again, N64
* there is a fog curve that we can set in Unity
	* setting it as exponential, as opposed to gradual or linear, can make the fog looks very dense
* like it starts almost at a very fixed spot

Silent Hill's fog looks more realistic
* but it's kinda the same thing
* it's still very tinted and very dense
* but it's also very fit its theme, making it look we are in some sort of desolate town

Fog is also used for dynamically load objects or to prevent rendering objects that are a certain distance away
* to optimize performance on a severely limited hardware
* at the time it was Playstation 1, a fairly weak console

Then we come to Shadow of the Colossus for PS4
* fog is still there, but it improves a lot, looks photo-realistic
* they probably have several layers of fog
* probably have textures and transparent objects that are simulating fog, fog that only hangs at a certain distance so it looks like fog going over the lake, etc.
* there's a lot of things here, but it's the same idea
	* they probably have the same foundational base fog present throughout the scene as well

> [!Key take-away]
> Fog can save performance. Very good for aesthetic as well if used probably.

___

# Unity 2D

> [!little tip]
> If we double click on game object in Hierarchy window we will zoom out to see the full object.
> It detects the resolution of our Scene window and place the camera accordingly.

There is a '2D' button in the Scene window
* which allows us to go between 2D mode and 3D mode

Canvas is a necessary thing for Unity to render UI stuff
* To create one, in the Hierarchy, Right click > UI > Canvas,
* or add any of the other things, and it will automatically add a canvas for us
* and an EventSystem is automatically added
	* it is how Unity talks to the canvas and all the UI elements
	* given mouse and keyboard inputs and the likes
	* not something that we necessarily have to worry about or use though, just need to have it be there
* in short, Canvas is the overall container for all the UI stuffs

If we want to add UI elements, like a Text, it needs to be a child of Canvas
* we can move it around, and it can snap to some places, so pretty handy
* also the component has many fields that we can manipulate and experiment

Ultimately, all these stuff is still 3D, but Unity presents it in a way that it looks and as if we're interacting with it in 2D

And Canvas and all of its UI stuff are seperated from the 3D world and thus from the Camera
* it just gets rendered on top of what ever the camera is looking at

* Camera has a Background and a Clear Flags
	* by default, it's the Skybox
	* but we can change it to a solid color
* It gets drawn to the screen before any geometry or object in the scene
___

## Anchors

often times we need to support multiple screen sizes and screen resolutions
* we can click on the box in Rect Transform, which is the anchor point selector, and then just choose the appropriate one and click, to change the anchor point
* then it will anchor to that position no matter the screen resolution

and in the Game window, we can change the Aspect Ratio
* to preview how things look in a certain screen ratio or even screen size
* there is a option that says 'Standalone', which is the default export size of our platform
___
