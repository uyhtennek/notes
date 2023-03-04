[Unite Austin 2017 - S.O.L.I.D. Unity](https://www.youtube.com/watch?v=eIf3-aDTOOA)
* by Dan
	* decent programming guy
	* have some free books
___

# Bob

Bob was told to make a game, and so he did
6 months later, the game is presented to some testers, and they don't like it
so Bob was requested to make changes

he is making new classes, then a lot of things need to change along
he is reusing some interfaces, but it's costly in time
he updated 1 line, only to discover 12 bugs
he is adding new features but they broke old ones

you would think this is what we do, this is programming
* problems come up when we're making architectures that solve problems from the current one

he tried to extend classes and things broke again
and, **nothing is testable**
* and, tests are very important
___

# SOLID Overview

```
SRP - Single Responsibility Principle
OCP - Open Closed Principle
LSP - Liskov's Substitution Principle
ISP - Interface Segregation Principle
DIP - Dependency Inversion Principle
```
___

# Dependency Inversion Principle
which is different from *dependency injection*

Bob has this class
```c#
public class Shot : MonoBehaviour {
	public float Damage = 1f;
}
```

and this `Damage` is being used all over the code
* `Enemy`, `BlastTrigger`, `Score`, ....
```c#
public class Score : MonoBehaviour {
	public float Points = 0f;
	public float DamagePointRatio = 0.1f;
	
	void ApplyShotDamageScore(Shot shot)
	{
		Points += DamagePointRatio * shot.Damage;
	}
}
```

and then later people requested a `RocketShot` thing
* it basically does the same thing as `Shot` except it has some few different things
```c#
public class RocketShot : MonoBehaviour {
	public float Damage = 1f;
}
```

But, all the other classes, `Enemy`, `BlastTrigger`, `Score`, ...., they all are looking at `Shot`
* they know `Shot`, they don't know `RocketShot`

so Bob tried to solve this
```c#
public class Score : MonoBehaviour {
	...
	
	void ApplyShotDamageScore(object shot)
	{
		if (shot is Shot)
		{
			Points += DamagePointRatio * ((Shot)shot).Damage;
		}
		if (shot is RocketShot)
		{
			Points += DamagePointRatio * ((RocketShot)shot).Damage;
		}
	}
}
```

it works, but certainly it's actually hiding more problems
* if we have more types of shot, we need to find all the place that expect a `Shot` again
* and it would be hard to find because functions are now expecting `object`
* since it's not clear, it added a chance for more bugs


## Implementation
bascially, use interfaces or abstracts

Bob now made an abstract class
```c#
public abstract class ShotBehaviour : MonoBehaviour {
	public float Damage = 1f;
}
```

now the other shot classes
```c#
public class Shot : ShotBehaviour {}
public class RocketShot : ShotBehaviour {}
```

then, the next time he adds a new shot, the other classes continue to work


## Sum up

DIP means
* Don't depend on *concrete*
	* concrete means very specific instances, like `Shot`
	* it should depend on `ShopBehaviour`

Interfaces are ideal
* but abstract class works as well

but since Unity Inspector does not support interfaces
* we almost have to use abstract classes
* if we want to make things designer friendly
	* generally we need to make things designer friendly
	* otherwise the designer has to come through you, the programmer
___

# Interface Segregation Principle

Bob is making an interface
* to let objects to be able to save and load themselves
```c#
public interface IContentIO
{
	string LoadAsString(string id);
	object LoadAsObject(string id);
	T LoadAs<T>(string id);
	byte[] LoadAsByteArray(string id);
	JsonObject LoadAsJson(string id);
}
```

now every time he adds this interface to another class
* he has to rewrite functions for the interface


## Implementation

an interface should only have one member, or one member purpose
* it should have one thing that it's doing
* only one reason to fail

also, it's easier for us to find the desired methods in Intellisense
* interfaces that follow ISP only have a few methods
* a function in a class can easily be hidden from hundreds of other methods and things

So, Bob separated `IContentIO` to five different interfaces
```c#
public interface IStringIO
{
	string LoadAsString(string id);
}

public interface IJsonIO
{
	JsonObject LoadAsJson(string id);
}

...
```

now a class can choose from any of these interfaces
* heck, even all five of them
	* we can have as many interfaces as we want to attach to an object


## Sum up

Use Interfaces

Keep everything small and focused
* even if you're putting in an abstract class, put an interface in front of that
	* and try to use the interfaces more than the abstract classes

A class can have a lot of interfaces

Interfaces are not supported in Unity Inspector
* so attach the interfaces to a class or an abstruct class

Internal methods can use interfaces
* we can use them in some framework that's running behind the scene

Save a lot of bugs because codes are small and focused
___

# Single Responsibility Principle

Bob made one changes, and 12 bugs showed up
* 1% of classes = 99% of work
```c#
public class Game : MonoBehaviour
{
	public float EnemySpeed = 1.0f;
	public float HeroSpeed = 1.0f;
	public float ParticleSmokeCount = 15.0f;
	...
	public int Level = 1;
	public float MaxHealth = 100f;
	public float Health = 100f
	public Texture[] SmokeParticles;
	public Enemy[] Enemies;
	...
}
```

Like, what on earth this class actually does?
* what fields should I use?
* what this method does?
* it costs time to find out

eventually, it introduces bugs
* because we can easily use the wrong stuff, or stuff will do unexpected things


## Implementation

A class should do only 1 thing
* only 1 reason to fail
* eg. a class that does ten things, that's not following this principle

Bob then split this big class to smaller classes
* introducing `UnitHealth.cs`, `UnitSmokeManager.cs`, `UnitMovement.cs`, ....
* we still need to structure our folders well because we would quickly find ourselves creating so many files to a point that it's hard to find things


## Sum up

In addition, it implies that this principle promote simple classes
* it's easy to figure things out and spot errors

In Unity, spliting classes isn't an issue because we can attach many tiny components on a object

If we use a mega class instead of multiple tiny classes, if we would like to change things it could easily break something that's completely unrelated
___

# Open Closed Principle

Bob started adding new features, and it started to break old ones
these are what Bob wrote
```c#
public abstract class ShapeBehaviour : MonoBehaviour
{
	public float Height { get; set; }
	public float Width { get; set; }
}

public class RectangleShape : ShapeBehaviour { }

public class ShapeTools
{
	public static float Area(ShapeBehaviour shape)
	{
		return shape.Height * shape.Width;
	}
}
```

it works, it follows some other principles, it's quite clean
until we add a circle shape
* notice that a circle's area is calculated with its radius but not width and height
```c#
public class CircleShape : ShapeBehaviour
{
	public float Radius = 2f;
}
```

but okay, it's not a big deal, Bob can fix this
```c#
public class ShapeTools
{
	public static float Area(ShapeBehaviour shape)
	{
		if (shape is RectangleShape)
		{
			return shape.Height * shape.Width;
		}
		
		if (shape is CircleShape)
		{
			var radius = ((CircleShape)shape).Radius;
			return radius * radius * Math.PI;
		}
		
		throw new System.NotImplementedException("The "
			+ shape.GetType().Name
			+ " is not implimented in ShapeTools.Area(...)");
	}
}
```

it works, and we would know exactly where to look if there is an error
* except that we don't
	* maybe this was the release version, players won't know how to fix a bug even you give them a nice error message
	* maybe this was distributed to a second developer in the form of a .dll
		* he doesn't have access to the source code


## Implementation

Once a class is written, we should not change it.
* ideally we already got unit tests or had testers tested things
* if we already got a class tested, then we change things in it, we are introducing a chance for that class to fail, and the tests ran mean nothing at that point
	* we need to rerun tests to validate our code again

So we shouldn't change a class, but we should still be able to expand it

So, first thing, we don't want `ShapeTools`
* we don't want to go back in and change the `Area` function everytime we introduce a new shape
* because it is already existed and already tested

But we still need `Area`, where should we put it?
```c#
public abstract class ShapeBehaviour : MonoBehaviour
{
	public float Height { get; set; }
	public float Width { get; set; }
	public abstract float Area();
}
```


## Sum up

we should be able to not change code, but expand code
___

# Liskov's Substitution Principle
actuall this is very close to Open Closed Principle

now Bob started extending classes, but things still broke somehow
* he got a class representing a 2D game, probably something like a board game
```c#
public class GameBoard : MonoBehaviour
{
	public GameTile[,] Tiles;
	public void SetTile(int x, int y, GameTile tile)
	{
		Tiles[x, y] = tile;
	}
}
```

then someone come in and say to Bob, "I want some part of the game to be 3D"
* since 90% of other codes is the same, Bob did this
```c#
public class GameBoard3D : GameBoard
{
	public GameTile[,,] Tiles3d;
	public void SetTile(int x, int y, int z, GameTile tile)
	{
		Tiles3d[x, y, z] = tile;
	}
}
```

again, the code can work, but the problem is
`SetTile` can be used for both `GameBoard` and `GameBoard3D`
* which is not ideal, `GameBoard3D` probably never want to use the 2D SetTile function, yet it can
* that is a chance for bugs
	* and this bug becomes harder to catch


## Implementation

If 2 different types have the same base type, they should both work for all the members that use the base type
* so, they should both work as if they are the base type

so Bob takes the `GameBoard3D` and take it back to `MonoBehaviour`
* because `GameBoard` and `GameBoard3D` is different enough
	* different enough that if we try to use `GameBoard3D` as `GameBoard`, things broke


## Sum up

we need to be able to trust whatever type it is as its base type
* eg. if a class inherits `string`, it should be able to operate as a `string`

we should never have to look at the source code of a class to understand what it does, or how it works in general
* eg. we shouldn't have to at it `GameBoard` to realize we can't use its `SetTile` on `GameBoard3D`
___

# Dependency Injection
again, it's not the same as Dependency Inversion

Dependency Inversion said things like
* eg. use `ShotBehaviour` or `IShotBehaviour` instead of `Shot`
* we want to operate against the interface or the abstraction, but not the concrete type

but, concrete still has to be constructed somewhere
* eg. we still would use and create `Shot`
	* after it, `Shot` can be treated as an interface or abstraction
* but there is no special handling for how we construct it

Dependency Injection solves this
* it says, "I don't want my class to know which one it's constructing"
* eg. if I have a DLL that has my whole game in it, and then someone else created this DLL plug-in for it, they should be able to overwrite `Shot` and put in their own `Shot`
* instead of saying `IGameBoard board = new GameBoard();`, it says `IGameBoard board = Di.Get<IGameBoard>();`

> [!info] Ninject
> Ninject is a common C# framework
> it works for .NET, but not Unity
> because Unity needs to control the constructor, unless we designed it specially

___

## Visual Studio Setup

this is optional because we can do things solely in Unity
* this is great though because building DLLs makes unit testing much faster and easier

1. Create a DLL in Unity or in Visual Studio

2. go to the properties of it
* we can choose which base library of Unity we want to use
* choose the right one according to your needs

....
___

## Implementation

```c#
public class Game : InjectBehaviour, IGame
{
	public IGameController GameController { get; set; }
	public GameConfig Configuration;
	...
	
	public Game()
	{
		Di.Bind<IGame>().AsSingleton(this);
	}
	
	void Start()
	{
		Di.Bind<IGameConfig>().AsSingleton(Configuration);
	}
}
```

```c#
public class DiBindings : IBindingConfiguration
{
	public int Order => 0;
	
	public void Setup()
	{
		Di.Bind<IBallController>().AsSingleton<BallController>();
		Di.Bind<IGameController>().AsSingleton<GameController>();
		Di.Bind<IMouseManagerController>().AsSingleton<MouseManagerController>();
	}
}
```

eg. game logic is completely separate from Unity
```c#

```
___

# Unit Test

We can do unit tests for all our game logic
* for the Views, the Unity stuffs, the MonoBehaviour and ScriptableObjects, we use TestRunner

why we prefer unit tests?
* it's small, it's the end unit
* an red light in TestRunner could due to many things
* an red light in an end unit can only due to one thing

we can mock Unity stuffs, or really the interfaces
* so tests can run in Visual Studio, we don't need to open up Unity to do our tests
___
