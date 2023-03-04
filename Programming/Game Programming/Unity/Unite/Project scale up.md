[Unite Berlin 2018 - From Pong to 15 person project - YouTube](https://www.youtube.com/watch?v=1le4vScG3gk)
___

# Our Struggle

1 man team
* Organizing lines of code

3 people team
* Software architecture

10+ people team
* Process automation
___

# Organize lines of code

at some point when we added enough features and changes to the project,
we'll realize, "this isn't working out"

it is better to untangle the mess, and reorganize to smaller pieces
* this cycle keep repeating over and over and over again
* the key point being, make sure you do clean things

**Demo: Pong**
* One thing = one prefab
* Logic for one thing = one MonoBehaviour
* a scene containing interlinked prefabs

It is perfectly fine, just not gonna work well in a larger scale
___

## Instance → Prefab → ScriptableObject

```c#
public class Padde : MonoBehaviour
{
	public string InputAxisName;            // different for each player
	public float MovementSpeedScaleFactor;  // shared for both players
	public float PositionScale;             // shared for both players
	...
}
```

1. Do you need something only once?
    Create a prefab, then instantiate.

2. Do you need something multiple times, with some instnace-specific modifications?
    Create a prefab, then instantiate, then override some settings.

3. Do you want to **ensure** the same setting across multiple instances?
    Create a ScriptableObject, then source data from there instead.

```c#
public class PaddleData : ScriptableObject
{
	public float MovementSpeedScaleFactor;
	public float PositionScale;
}
```

```c#
public class Paddle : MonoBehaviour
{
	[Header("Static Data")]
	public PaddleData PaddleData;
	
	[Header("Customizable settings")]
	public string InputAxisname;
	
	// private vairables
	...
}
```

The intention behind the settings is much clearer this way.
___

## Splitting up MonoBehaviours

> [!important] Single Responsibility Principle
> Each class should handle one single thing.

If it is executed well, we can give a short description of the class
* what does it do?
* what does it **not** do?

It is a golden rule no matter the size of the project.

```c#
public class Paddle : MonoBehaviour
{
	[Header("Static Data")]
	public PaddleData PaddleData;
	
	[Header("Customizable settings")]
	public string InputAxisName;
	
	private float yPosition;
	private MeshRenderer mesh;
	private AudioSource bounceSfx;
	
	void Start()
	{
		mesh = GetComponent<MeshRenderer>();
		bounceSfx = GetComponent<AudioSource>();
	}
	
	void Update()
	{
		float delta = Input.GetAxis(InputAxisName) * PaddleData.MovementSpeedScaleFactor * Time.deltaTime;
		yPosition = Mathf.Clamp(yPosition + delta, -1, 1);
		
		transform.position = new Vector3(transform.position.x, yPosition * PaddleData.PositionScale, transform.position.z);
	}
	
	private void OnTriggerEnter(Collider other)
	{
		bounceSfx.Play();
	}
}
```

```c#
public class Ball : MonoBehaviour {
	[Header("Customizable per instance")]
	public Vector3 Velocity;
	
	void FixedUpdate()
	{
		transform.localPosition = transform.localPosition + Velocity * Time.fixedDeltaTime;
	}
	
	void OnTriggerEnter(Collider collider)
	{
		if (collider.gameObject.tag == "HorizontalWall")
			Velocity = new Vector3(Velocity.x, -Velocity.y, Velocity.z);
		if (collider.gameObject.tag == "VerticalWall")
		{
			Destroy(gameObject);
		}
		if (collider.gameObject.tag == "Padde")
			Velocity = new Vector3(-Velocity.x, Velocity.y, Velocity.z);
	}
}
```

Notice, the variable `Velocity` has 2 purpose:
1. it is the initial value
2. it is the ball's current velocity
* As soon as the ball moves, we lose information about the initial velocity

It uses a `Destroy(gameObject)`
* remember, a piece of code that deletes the owner of itself, is not something we see very commonly in large code bases
	* because we would prefer owners to delete things that they own

Notice the same MonoBehaviour actually has many functionalities
* Some can be put into other MonoBehaviours
* Some can be moved into ScriptableObjects
* Some even can be in raw C# classes


### ScriptableObjects or C# classes?
Now we come to a point, where we need to choose how to decouple the script

ScriptableObjects
* allows using Unity Inspector to view, edit and link together the code and data
* see other talks
	* [Unite 2016 - Overthrowing the MonoBehaviour Tyranny in a Glorious Scriptable Object Revolution](https://www.youtube.com/watch?v=6vmRwLYWNRo)
	* [Unite Austin 2017 - Game Architecture with Scriptable Objects](https://www.youtube.com/watch?v=raQ3iHhE_Kk)

Regular C# classes
* have better language facilities than Unity's own objects
	* so code can be broken down into smaller chunks
* can be moved outside of Unity
	* can be shared with native .NET code bases
	* work better for backend
* the downside is that Unity doesn't understand what we're doing with the raw C# class
	* so we need to do the wire up
	* we need things like logging and using the debugger to inspect what's going on
___

### Moving into C# classes

Principles
* Split up logic for different responsibilities into different classes
* Anything that doesn't need to be in MonoBehaviours, should not be in MonoBehaviours

```c#
public class Ball : MonoBehaviour {
	[Header("Customizable per instance")]
	[FormerlySerializedAs("Velocity")]
	public Vector3 InitialVelocity;
	
	private BallLogic ballLogic;
	private BallSimulation ballSimulation;
	
	void Start()
	{
		// controls how the ball moves
		ballSimulation = new BallSimulation(InitialVelocity);
		// contain the logic of the ball
		// (how to behave when collided with something)
		ballLogic = new BallLogic(ballSimulation);
	}
	
	void FixedUpdate()
	{
		transform.localPosition = ballSimulation.UpdatePosition(transform.localPosition, Time.fixedDeltaTime);
	}
	
	void OnTriggerEnter(Collider collider)
	{
		bool isAlive = ballLogic.Hit(collider.gameObject.tag);
		if (!isAlive)
			Destroy(gameObject);  // this is not so deep in many layers now
	}
}
```

Now the Ball MonoBehaviour only does
1. Declare an initial velocity
2. Destroy itself
___

### Using Interfaces
the reorganized code, is not perfect at all

Notice, `Ball` and `BallSimulation` feels not tied so tightly
* `BallSimulation.UpdatePosition` just update whatever gets passed in to it
* the line `transform.localPosition = ballSimulation.UpdatePosition(transform.localPosition, Time.fixedDeltaTime);` is kinda stupid....

```c#
public interface ILocalPositionAdapter
{
	Vector3 LocalPosition { get; set; }
}
```

```c#
public class Ball : MonoBehaviour, ILocalPositionAdapter {
	[Header("Customizable per instance")]
	[FormerlySerializedAs("Velocity")]
	public Vector3 InitialVelocity;
	
	public BallLogic BallLogic { get; private set; }
	private BallSimulation ballSimulation;
	
	public Vecotr3 LocalPosition
	{
		get { return transform.localPosition; }
		set { transform.localPosition = value; }
	}
	
	void Awake()
	{
		ballSimulation = new BallSimulation(InitialVelocity);
		// note this access Ball's transform.localPosition directly
		// and it access ONLY the LocalPosition, or the ILocalPositionAdapter portion
		BallLogic = new BallLogic(this, ballSimulation);
		// also it calls a function for destroying the ball, instead of raised a flag
		BallLogic.OnDestroyed += () => Destroy(gameobject);
	}
	
	void FixedUpdate()
	{
		BallLogic.FixedUpdate(Time.fixedDeltaTime);
	}
	
	void OnTriggerEnter(Collider collider)
	{
		BallLogic.Hit(collider.gameObject.tag);
	}
}
```
___

# Software architecture
if you execute the above principles well, you'll be fine, on a one-person project


## Hierarchy

if we only have MonoBehaviours to speak with other things
there is no hierarchy to speak of

if we separated MonoBehaviour and Logic, as the above does, we constructed some hierarchy here
```
MonoBehavior
> Logic
> > Input
> > Simulation
```

although the hierarchy is quite neat this way,
often times it breaks down when Presentation comes in to play
* eg. spawning special effects, updating scoring counter, etc.
* somehow the Presentation needs to know what's going on in the Logic
	* because we don't want the Logic to read the Presentation to spawn or update things

> [!tip]
> In an ideal world, it should be possible to run our game logic, without any presentation. The logic should not depend on the existing of the our game.
> 
> We should effectively be able to run our game in 2 modes:
> Logic only; and Logic + Presentation.


## Data-only classes
Sometimes a thing is juat a collection of data.


## Helper classes
Likewise, it is perfectly fine to have a class that doesn't own any data but only functions.


## Static methods
more functional programming → less bugs
* better than regular stateful typical object-oriented programming
* [In-depth: Functional programming in C++ (gamedeveloper.com)](https://www.gamedeveloper.com/programming/in-depth-functional-programming-in-c-)


### Decouple your objects
back to the example of the Logic and Presentation

somehow the Logic needs to manipulate the Presentation, or
somehow the Presentation needs to read from the Logic

maybe it is good to create something in between
* can be Buffers, Queues, Message Buses


#### Messaging
core principle is that neither receiver or sender knows the other party, but both of theme know there is a message system
* Unity has the UnityEvents
* we can write our own event system as well
	* [A Type-Safe Event System for Unity3D – Will R. Miller (willrmiller.com)](http://www.willrmiller.com/?p=87)
___

# Scene loading

## Stop using `LoadSceneMode.Single`
use `LoadSceneMode.Additive` instead

* use explicit unloads when you want to unload a scene
* sooner or later you will need to keep a few objects alive, during a scene transition


## Stop using `DontDestroyOnLoad`
flagging things is generally bad

* it is hard to use because there is no `UnDontDestroyOnLoad`
* if you load scenes additively, we have long-lived scene instead of long-lived object
	* which is easier to handle


## Have a minimal boot scene
it reduces the amount of time it takes, between application startup and us getting control of things

* we want to be able to show a loading screen pretty quickly, pretty early on
* how things loaded is in our control


## Support a clean, controlled shutdown
just like how we should have control over how things start up in a particular order,
we also want a particular order through which it shut things down

* it is great for finding errors
* it gives us opportunities for finding out if it is leaking things
	* eg. as we shut down the game, we unload everything except the boot scene, then it is a perfect place to check if there are memory leaks throughout the entier application life cycle
* also good for exiting Play mode in Unity Editor
	* our Editor things or Editor scripts are less likely to show weird things


## Example scene layout
![[clean-scene-layout.png|510]]


## Scene file merge pains

* use a real version control system
	* like always use the scene from the main branch
* store all assets as text
* move objects out of scene files, by making them as prefabs
* split scene files input multiple smaller scenes
	* eg. a game scene is splitted into an environment scene, a HUD scene, a logic scene, etc.
	* it was worth it
___

# Process automation
for >10 or >15 people teams

> [!important]
> Leave all the repetitive work, to the automated dumb things.


## Test the code
since we separated logic and presentation, we can build tests for our code
* the outcome is another programmer isn't so afraid to touch our code


## Test the content
validate things about the content or the assets
* problems found before starting the game, saved time


## Automated building


## Automated playthroughs
* making some kind of AI that can play the game
* let AIs find bugs for us
___
