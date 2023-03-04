[Overthrowing the MonoBehaviour Tyranny in a Glorious Scriptable Object Revolution](https://www.youtube.com/watch?v=6vmRwLYWNRo)
* [Project demo](https://github.com/richard-fine/scriptable-object-demo)
___

# The MonoBehaviour Tyranny
chaotic

we want shared state to get outside of MonoBehaviour

MonoBehaviour is bad for collaboration
* because it attaches to GameObject

it has callback chaos
* create race condition
___

# Uninstantiated Prefabs
this was a solution, but it's a bad hack

it's confusing what should be in the scene and what shouldn't
___

# C# statics
this was another solution

but it's not userfriendly
* no serialisation
* no visulisation in the Inspector
* lotst on domain reload

too DIY as a result
___

# ScriptableObject
"It's MonoBehaviour, but it's not a component"

They can't be attached to GameObjects

They only have 3 callbacks
* `OnEable`, `OnDisable`, `OnDestroy`
* which is good because they don't do extra thing

Can be serialised and visualised
___

# How to ScriptableObject

declare
```c#
public class MySO: UnityEngine.ScriptableObject
{
	public int someVar;
}
```

reference
```c#
public MySO mySOInstance;
```

create instances in-memory with
`ScriptableObject.CreateInstance<MySO>()`

bind them to .asset files with
`AssetDatabase.CreateAsset()` or
`AssetDatabase.AddObjectToAsset()`

the above 2 step can be automated by
`[CreateAssetMenu]`


## ScriptableObject Callbacks

`OnEnable()`
* when object is created or loaded
* also, when scripts get re-compiled
	* because they need to warm up

`OnDisable()`
* the opposite

`OnDestroy()`
* happens when it's destroyed


## ScriptableObject LifeCycle

Same as any other asset
* won't get deleted until we do so

Can be deleted from `Resources.UnloadUnusedAssets()` if it's not being referenced
___

# Destroy / DestroyImmediate

Unity is a C++ engine, not a .NET engine
* it has a .NET layer on top of the C++ foundation

therefore, an object in Unity actually has dual identity
* parts of it live in the C# part
* parts of it live in the C++ part
	* basically Unity took care of this part for us

when we call `Destroy()`, we might think that it kills the whole thing, it's not true
* when we call `Destroy()`, what actually happens is it kill the C++ part
	* we cannot kill the C# part
* the only way to kill the C# part, is to make sure that nothing is referencing it
	* and then let the GC runs

so, when you're destroying things, it's a good idea to also null out the references
* because even if something appears to be null, it might not be
* in Unity, it pretends an object to be null when the C++ part is destroyed
	* for some reason, leading to both good and bad things
___

# Patterns
___

## Data Objects
and Tables

eg. drop items and drop rates of an enemy

people sometimes use a real database for these kind of stuffs
* but SO is less of an overhead
* UX is smoother as well
___

## Extendable Enums

we can have empty SOs bound to asset files
* even though it's empty, we can compare it
* so we can use them as Enums

similarly, we can use them as dictionary keys

the key advantage is that we can add new values to the enum without changing any code

surely these can be non-empty, it's natural
* and it's easy to add extra data to it

and it supports `null`
___

## Dual Serialisation

ScriptableObjects are supported by JsonUtility
* so it works well with JSON

eg. we have a simple level editor
```c#
class LevelLayout : ScriptableObject
{
	public Vector2[] platformPositions;
	public Vector2[] enemyPositions;
	public Vector2[] coinPositions;
	public Vector2 startPosition;
}
```

and then when we want to load it
```c#
// Load built-in level from an AssetBundle
level = lvlsBundle.LoadAsset<LevelLayout>("lvl1.asset");

// Load level from JSON
level = CreateInstance<LevelLayout>();
var json = File.ReadAllText("customlevel.json");
JsonUtility.FromJsonOverwrite(json, level);
```

with some extra effort, we can even not care whether it's built-in or JSON
___

## Reload-Proof Singletons

when we exit play mode, all the C# parts are gone
* the data though, still live in the C++ parts
* so we can load the data back into our ScriptableObjects after exited play mode

```c#
class MySingleton : ScriptableObject {
	private static MySingleton _inst;
	public static MySingleTon Instance {
		get {
			if (!_inst)
				_inst = Resources.FindObjectOfType<MySingleton>();
			if (!_inst)
				_inst = CreateInstance<MySingleton>();
			... // set a hide flag so that GC won't clean it up
			return _inst;
		}
	}
}
```
___

## Delegate objects

allow strategy pattern

```c#
abstract class PowerupEffect : ScriptableObject
{
	public abstract void Apply (GameObject collector);
}

public void OnTriggerEnter (GameObject toucher)
{
	Destroy(gameObject);
	powerupEffect.Apply(toucher);
}
```

```c#
public class HealthBuff : PowerupEffect {
	public float amount;
	public override void Apply(GameObject collector) {
		collector.GetComponent<Health>().value += amount;
	}
}
// or
public class InstaKill : PowerupEffect {
	public GameObject explosionFX;
	public override void Apply(GameObject collector) {
		Instantiate(explosionFX, collector.position, collector.rotation);
		Destroy(collector);
	}
}
```

note that we separate these power-up effects to ScriptableObjects
* we can certainly do these things in MonoBehaviour
* but it wouldn't be as clean
	* not appreciating separation of concerns
___
