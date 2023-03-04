[Unite Austin 2017 - Game Architecture with Scriptable Objects - YouTube](https://www.youtube.com/watch?v=raQ3iHhE_Kk)
___

# Engineering Pillars

* Modular
* Editable
* Debuggable
___

## Modular
Systems should not directly dependent on each other

Scenes are clean slates
* we shouldn't have transient data that exists between scenes
	* minimize `DontDestroyOnLoad` objects
	* every time we hit a scene, it's a clean break or clean load

Prefabs work on their own
* all the functionalities are contained inside of the prefab itself
* helps version control

Components!
* break things up as much as possible
* make it do one thing, and only one thing
___

## Editable

Focus on data
* data-driven

Change the game without changing code

Emergent Design
* since every thing is a component, we can just rearrange things, change some settings, to make new game mechanics

Change at runtime
* super important for iteration
* the more we can change our game in runtime, the more we can balance and find values
___

## Debuggable

Test in isolation (modularity)

Make debug views and features
* never consider a feature done, until we have some plan for debugging it

Never fix a bug you don't understand
* "you have to follow the bug, understand it, get to know its family. and then kill it in front of its family."
___

# Scriptable Object Basics

**Primer**
* it's a serializable class in Unity
* similar to MonoBehaviour, but no Transform and GameObject
	* MonoBehaviour and Scriptable Object, under the hood, are actually the same exact type on the C++ layer of the engine
* typically it's saved as .asset file
	* it's not necessary though
	* we can just leave them in memory and work with them there
		* with `CreateInstance`, at runtime
	* we can actually save more than one scriptable object into a .asset file


## Simple Use Cases

Using as a massive game config file
* some other objects, sounds, etc., are stored in a scriptable object

Inventory

Enemy stats

AudioCollection
___

# Architecture
Almost all games have the same problem: **Singletons**

why use Singleton though?
* access anything from anywhere
* persistent state
	* using `DontDestroyOnLoad`
* easy to understand
* consistent pattern
* easy to "plan"

why it is bad then?
* Rigid connections, not modular
	* one thing depends on the existence of another thing
* no polymorphism
	* we can't really get the child classes
* not testable
* dependency nightmare
* Single Instance!

## Solutions
* reduce need for global managers
* inversion of control
	* this is a software design principle
		* eg. a very common case is Dependency Injection
	* objects are given dependencies, the things that they need to work with
		* rather than they going out and getting them
		* eg. something in the game need to feed stuff to some other things
			* "Hey you, UI piece, here is the string you should display, don't ask any thing."
	* Single responsibility principle
___

## Modular Data
eg. we have 20 enemies that need to know what speed they move

1. Just set the speed in the prefab
* really annoying to do

2. ask for `EnemyManager.Instance.MoveSpeed`
* Hard reference
* Dependency on manager being loaded
	* Null Reference every where.....

3. `EnemyConfig` ScritableObject reference
* [Richard Fine talked about this](https://www.youtube.com/watch?v=6vmRwLYWNRo)
* having a set of variables that are associated with the enemies, as a scriptable object
* but it lumps all part of the enemy stats together
	* eg. we added a new ranged enemy, it needs to know what's its minimum and maximum fire distance
	* it's now a new variable type, that can't be added to this large definition
* so it limits extension or modularity

4. Maybe breaking it to smaller different scriptable objects
```c#
[CreateAssetMenu]
public class FloatVariable : ScriptableObject
{
	public float Value;
}
```

but what's this `Value` about?
* could be anything, Hitpoints, Damage Amount, Timing Data, etc.
* use the name for asset to indicate what is it about

```c#
public class DumbEnemy : MonoBehaviour
{
	public FloatVariable MaxHP;
	public FloatVariable MoveSpeed;
}
```

this workflow can be annoying though
* it is quite cumbersome, right?

```c#
[Serializable]
public class FloatReference
{
	public bool UseConstant = true;
	public float ConstantValue;
	public FloatVariable Variable;
	
	public float Value
	{
		get { return UseConstant ? ConstantValue : Variable.Value; }
	}
}
```

```c#
public class DumbEnemy : MonoBehaviour
{
	public FloatReference MaxHP;
	public FloatReference MoveSpeed;
}
```

then make some editor tool like this
![[SO-FloatReference.png]]
___

### Demo

![[SO-PlayerHP.png]]

we have a structure like this
* Player reads and updates PlayerHP
* Other things just read from PlayerHP

Note that, we don't even need a Player, to test HealthUI, AudioSystem and EnemyAI
* so we don't really need to worry about breaking things

these form of data, has some sort of persistent state
* they persist from scene to scene
* very useful for like PlayerHP
___

# Event Architecture
often times we don't care about the data, but just care about if something happened

* modularize systems
	* so we can use the modules in other projects
	* so we can isolate prefabs
		* a prefab can respond to an event, and not knowing where it's coming from
* a little bit of optimization
	* in the above demo, a Health meter is **constantly** reading PlayerHP
		* but in many cases, we just want to fire an event every time the player loses HP
* highly debuggable
	* if we can invoke those events through the editor or through other systems or any thing
	* we can check "if the event is raised, is the behaviour correct? and is the event being raised?"
		* we can even do a binary search to hunt down where the bugs coming from
___

## UnityEvent
it is like a *serialized function call*

**advantages**
* Editor hook-ups
* Less code, fewer assumptions
	* our code doesn't assume where the call is coming from
* Pass Arguments
	* we can extend it by using `UnityEvent<T>`
	* we get static and dynamic arguments
		* static arguments are set in the inspector
		* dynamic arguments are passed in code

**problems**
* rigid bindings
	* button hard references what responds to it
* limited serialization
	* we can define UnityEvent that takes up to four arguments, they can be any type
	* but we can only edit one argument for the UnityEvent in the Inspector, and it must be int, bool, float, ...., or `UnityEngine.Object`
* garbage allocation
	* since it is a serialized function call, it is using a string to resolve things
	* so every time we call that we're gonna see a little bit of garbage
		* they are still useful, but we would not want to call it every frame
			* something happens every frame is shouldn't be an event any way
___

## Solution
maybe we can try making our own events

* data-driven
* designer authorable
* debuggable

```c#
[CreateAssetMenu]
public class GameEvent : ScriptableObject
{
	private List<GameEventListener> listeners = new List<GameEventListener>();
	
	public void Raise()
	{
		// loop it backwards because in case an event removes itself or others
		for (int i = listeners.Count - 1; i >= 0; i--)
			listeners[i].OnEventRaised();
	}
	
	public void RegisterListener(GameEventListener listener) ...
	public void UnregisterListener(GameEventListener listener) ...
}
```

```c#
public class GameEventListener : MonoBehaviour
{
	public GameEvent Event;
	// all the complex things go to here, like feedbacks
	public UnityEvent Response;
	
	private void OnEnable() {
		Event.RegisterListener(this);
	}
	
	private void OnDisable() {
		Event.UnregisterListener(this);
	}
	
	public void OnEventRaised() {
		Response.Invoke();
	}
}
```

In the prefab, we can drag and drop the event we want it to respond to
And then use an UnityEvent to serialize the function call or calls as the response


### Demo

also we can make an editor tool to trigger the events.
* for debug sake

sometimes we can fix bug without writing code this way
* which is much less stressful
___

# Singleton Goals

an EnemyManager would want to
* track all enemies in a scene
* understand player, like where he is
* issue commands

**problems**
* Race conditions
	* EnemyManager is called before it is created, but called by an Enemy or something
	* force us to do null reference or some other stuffs
* Rigid singleton
	* we can't make a test scene with only the enemy we want to test, without bringing in all the other systems
* Only one
___

## Runtime Sets
it is almost exact like the [[#Modular Data|Float Variable]], except it's a list but not a float

* it keep tracks of a list of objects in the scene
* avoid race conditions
	* because scriptable object always in memory before any our game stuffs
* more flexible than Unity tags and Singleton
	* btw, we can only have one tag on an object, that's kinda more like a label than a tag

```c#
public abstract class RuntimeSet<T> : ScriptableObject
{
	public List<T> Items = new List<T>();
	
	public void Add(T t)
	{
		if (!Items.Contains(t))
			Items.Add(t);
	}
	
	public void Remove(T t)
	{
		if (Items.Contains(t))
			Items.Remove(t);
	}
}
```


### Use cases

eg. RTS game
* have a list of the places we built
* have a list of walking units, and another list for flying units

have a list of renderers for special effects
* then when the camera renders, it just go through the list

items on a map
* anything we want to display on the mini-map, we can add them to a list
* then our UI component can inspect the list and draw


### Demo

we can have things to register themselves to a Scriptable Objects
* and then we can do all kind of stuff with that Scriptable Objects with that data
___

# Enums

**limitations of enums**
* it is code-driven, we must change it in code
	* btw, the more code we can remove from the game, the less bugs we're going to have
	* if we make something as enum, and we later need to add a feature related to it, it's a pain
	* if it is working with something fixed by some law, it can work well
		* eg. cardinal directions must be North, South, East, West
		* plus, we can work with enums using comparison operators `==, !=, >, >=, ...`
* difficult to remove or add or reorder things
	* in Unity, enums are serialized based on their index
		* and therefore saved out based on their index
		* if we deleted one from the enum, it breaks every single thing used the enum
			* some games deal with this by renaming something to "Unused"
* can't hold additional data
	* we end up building sort of a lookup table in a lot of cases
	* although sometimes we might think we don't need it to hold additional data, it is actually not a good design


## Solution Demo

in a rock paper scissor game, we can assign an object, an "element"
* which is either rock, paper, or scissor
* it is a scriptable object, defined what other element that can beat
	* other elements are also represented as Scriptable Objects

now we can add a Dynamite element, which defeat everything
* we don't need to touch code for this. it's just a new scriptable object.
___

# Asset Based Systems
how to pack entire system into an asset

we can have Scriptable Objects to reference other Scriptable Objects
* eg. we have an Inventory Scriptable Object, which is all of the Player's equipment
* the Player, Equip Screen, NPC, game objects reference Inventory
* then the Inventory references the Save System, or the Localisation
	* so that the Inventory is more than just some data
	* it can have more functionality
___
