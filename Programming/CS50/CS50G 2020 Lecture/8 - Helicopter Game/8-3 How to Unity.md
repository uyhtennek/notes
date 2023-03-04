# Colliders and Triggers

Unity shows us visually where a lot of the things are in the game world
* Green Boxes are representing an object's Hitboxes
	* although they are called *colliders*

Collider is literally just something that collides with something else
* we can set it to be a trigger or not
* when a trigger detects a collision with a collider that's **not** a trigger, it calls the `OnTriggerEnter()` function

We separated the colliders for the Helicopter's blades and its body
* the core reason being the 2 parts are different in size
	* the blades are longer than the body
	* so the collider is more fair
* one nice thing about box colliders, is that they're not very expensive
	* they're just boxes after all
* another small nice thing is that if our model is in box shape, the assigned box collider would just scale the right way so that it's the same size as the model
	* if we have model that is in other shapes, the box collider would just be as big as it can completely encapsulate it
		* which would be a little bit unfair for our helicopter game

* spheres aren't bad as well
* capsules are easy too
	* common for being the player's or characters' collider
	* because characters usually have rounded heads

> [!info]
> We can take a complicated model or object, then break it down in a way that, not in terms of what it looks like to be collided with, but how we can simplify the collision detection.
> 
> Like sometimes we model a character's arms, body, legs as cylinders or boxes, rather than complicated geometry.
> 
> We usually don't want to do peer geometry collision for anything, because that gets really expensive. We aim for simple shapes to be our colliders.

We can easily give a game object a component
* Inspector (= component inspector) > Add Component

```c#
// file: Coin.cs
void OnTriggerEnter(Collider other) {
	// the function will look for other collider that triggers the callback
	// the collider that's colliding with this object, with this coin
	
	// in our game only the Helicopter has a non-trigger collider
	other.transform.parent.GetComponent<HeliController>().PickupCoin();
	Destroy(gameObject)
}
```

`other.transform.parent`
* because the colliders of the Helicopter object are children of the actual Helicopter object
	* which contains the component `HeliController`

`GetComponent<HeliController>()`
* it's a function that will look through all of the components on a game object
* we specifies the component that we want
	* in this case `HeliController`
	* angle brackets `<>` here, are the *generic type specifier*
		* we can pass in any Type for it to have the function looking for the passed in Type
* so we do have some sort of type flexibility in C#
	* but we have to go the extra mile to specify it and implement functions that can take generic arguments like this
	* similar to Java and C++

`PickupCoin()`
* a function that's part of `HeliController`
* increments the coin counts and triggers a particle effect

`Destroy(gameObject)`
* `gameObject` = this coin that's executing this function

```c#
// file: HeliController.cs
...
public void PickupCoin() {
	coinTotal += 1;
	// since audio source can have multiple audio files?
	// we have more than one AudioSource?
	GetComponents<AudioSource>()[0].Play();
	GetComponent<ParticleSystem>().Play();
}
```

```c#
// file: Skyscraper.cs
void OnTriggerEnter(Collider other) {
	other.transform.parent.gameObject.GetComponent<HeliController>().Explode();
}
```
___

# Prefabs

The coins, the skyscrapers, the airplanes, they are all "prefabricated" assets that we have gotten ready, ready for spawning into our scene

Notice, before we play the game, it doesn't have any coin, skycraper, or airplane
But it does have a CoinSpawner, SkyCraperSpawner, AirplaneSpawner
* they are all empty game objects
* but have a spawner script associated

```c#
// file: AirplaneSpawner.cs
public GameObject[] prefabs;

void Start() {
	StartCoroutine(SpawnAirplanes());
}

IEnumerator SpawnAirplanes() {
	whilte (true) {
		Instantiate(prefabs[Random.Range(0, prefabs.Length)], new Vector3(26, RandomRange(7, 10), 11), Quaternion.Euler(-90f, -90f, 0f));
		yield return new WaitForSeconds(Random.Range(3, 10))
	}
}
```

`Quaternion.Euler(-90f, -90f, 0f)`
* the math behind 3D rotation is pretty complex
* `Quaternion.Euler()` allows us to think in degrees
	* to perform a 3D rotation on something fairly easily
* `Quaternion.identity` means the default rotation
	* whatever rotation is set as in the prefab

`Instantiate()`
* a global function, which we can call anywhere
* we need to give it a actual object or prefab that it knows how to instantiate


## Creating a prefab

Just create the object in the scene, then drag it into the Project window
* then we can just drag the prefab from the Project window into the scene, to add a new game object from that prefab

Prefab has some kind of identifier to Unity, but hidden from us
* it's not something we'd worry about in our actual code though
___

# Unity Inspector GUI

> [!tip]
> Any field with a `public` access identifier, can be seen and changed value in Unity Editor > Inspector.
> 
> Many of the data type have customized views for them in the Inspector.
> eg. a `Color` data type would have a Color Picker view in Inspector.
> 
> Also this is one of the strengths of Unity, to be able to customize not only just components but also the views for the components and its fields.

1. we create the class first, to create a new component
2. we give it the public field that it needs
3. the Editor reads the scripts, looks through all the public fields
4. then it will create the necessary GUI elements in the Editor
	* which can interact with the code
___

# Coroutine

`StartCoroutine(SpawnAirplanes())`
* Coroutine is effectively a function, that yields control every time it's called
	* when it runs, rather than just go through all of its code and then return
	* it'll actually yield, pausing the code for some length of time
		* when we use the keyword `yield`
* we need to call `StartCoroutine()` to trigger a coroutine function
	* which needs to return a Type `IEnumerator`

`while (true)`
* usually it's not something that we want in a game, because it'll just lock the game forever
* but since it's in a Coroutine, that will yield control back to whatever called it
	* for some length of time or depending on how we've programmed it
* it doesn't lock the game

`yield return new WaitForSeconds(Random.Range(3, 10))`
* yields for some seconds and then continuese its execution
* `WaitForSeconds()` is a global object, and a asynchronous object
	* allow us to yield some length of time, in seconds
* the function is returning `new WaitForSeconds()`, and pass it to the `yield` instruction
	* then later we'll be back to where we yielded
	* it's a weird syntax, but we have to do things their way

if we don't want to use Coroutine,
we can make a simple update timer as well
* but Coroutine takes out the need to have something that we keep track of with a timer

> [!notes]
> We don't need to necessarily know all the low-level details. But just know if you want asynchronous behavior, your function needs to return an IEnumerator.
> 
> Then C# knows it's a generator function which will yield control for some time. And it will manage that for us.

Other spawners are similar
```c#
// file: CoinSpawner
IEnumerator SpawnCoins() {
	while (true) {
		int coinsThisRow = Random.Range(1, 4);
		
		for (int i = 0; i < coinsThisRow; i++) {
			Instantiate(prefabs[Random.Range(0, prefabs.Length)], new Vector3(26, Random.Range(-10, 10), 10), Quaternion.identity);
		}
		
		yield return new WaitForSeconds(Random.Range(1, 5));
	}
}
```

```c#
// file: SkyscraperSpawner.cs
public GameObject[] prefabs;
// this drives the scroll speed of the skyscrapers, the coins, and the airplanes
public static float speed = 10f;
...
IEnumerator SpawnSkysscrapers() {
	whilte (true) {
		Instantiate(prefabs[Random.Range(0, prefabs.Length)], ...)
		
		
	}
}
```

`public static float speed = 10f;`
* `static` means no matter how many skyscraper spawners we created, they're all going to share this static field, `speed`
	* it's always the same amount across all instances
	* since there is only 1 `speed` field across all instances
* so `static` means it's part of the class
	* it belongs to the `SkyscraperSpawner` class
	* but not belongs to the `SkyscraperSpawner` object

```c#
// file: Airplane.cs
void Update() {
	if (transform.position.x < -25) {
		Destroy(gameObject);
	}
	else {
		transform.Translate(-SkyscraperSpawner.speed * 2 * Time.deltaTime, ...)
	}
}
```

for `Coin.cs`, it's the same thing
___

# Better design

we can see there are actually quite some places have duplicated code,
so how can we better engineer that?

Coins, skyscrapers, and airplanes are quite similar
* maybe we should have a `Scrollable` component
	* put all the related logics there
___

# Infinite Scroller

Like [[1-1 Setup Stages|Flappy Bird]], the helicopter doesn't really move
* it's the background and obstacles that are moving

the background is a plane with texture, then how do we scroll the background?
* we are changing the texture or scrolling the texture

> [!notes]
> The plane is one-sided. Meaning it doesn't get rendered if we look at it from the back.
> Usually polygon has one side that gets shaded and one side that does not get shaded.
> So that's not a bug, that's a feature.

when we take a texture and apply it to a 3D surface
* it's basically mapping the texture coordinates to the 3D coordinates of the object
	* called *UV mapping*
		* UV because usually when we're dealing with a 3D world, we got XYZ
		* then we want to think of the texture separately from the 3D objects, so we use UV instead of XY

* so there is an offset field of the texture that we can set
	* in the material that holds the texture
* that would reposition the UV mapping of that texture onto the 3D surface
* and when we offset texture, it will just wrap itself around
	* so we don't need to worry about how to implement looping the texture
		* like how we did for [[1-1 Setup Stages#bird-1|Flappy Bird's background]]

```c#
// file: ScrollingBackground.cs
public float scrollSpeed = .1f;
public Renderer rend;

void Start() {
	rend = GetComponent<Renderer>();
}

void Update() {
	// Time.time, time passed since the beginning of the game has started
	// Time.deltaTime, time passed since the last frame
	float offset = Time.time * scrollSpeed;
	// shift the UV mapping of the texture to the 3D object, or material
	rend.material.SetTextureOffset("_MainTex", new Vector2(offset, 0))
	rend.material.SetTextureOffset("_BumpMap", new Vector2(offset, 0))
}
```

Any 3D object has a renderer, so any object with a 3D model
Every renderer has a material associated with it
Material doesn't necessarily have a texture though
* in our case, it does, and it's called `"_MainTex"`, which is the default
* material can just be a color, often, but it can also be a color and also a texture

It doesn't matter what texture we gives to a material
* the material will calculate, based on the mesh, how to draw the texture best

Vectors are just combinations of numbers
* can be however many we want

Bump Map, in short, pretends as if our 3D mesh has a bunch of bumps in it
* if we were to model every single tiny little bump in a model surface, it'd get extremely difficult to render large objects
	* there would be thousands of polygons
	* so we'd use a bump map
* it will effectively skew the surface normals of a 3D mesh, during lighting
* it's only for the lighting
	* only when it gets lit, gets shaded, it can look bumpy
* and we're still dealing with a completely flat and simple mesh, at least in this case

We actually didn't have a Bump Map on the texture
* we set the bump map's offset anyway
* just so we don't need to revisit the code if we later do add a bump map to it
___

# Audio

`AudioSource` plays audio
`AudioListener` listens for audio, then plays it back to the user's speakers

We can have infinite audio sources, but ideally one audio listener
* usually it's the main camera

Sounds in Unity can be 2D or 3D, specified as `Spatial Blend`
* that affects based on where the sound is, how it sounds in the game space
	* based on how far away it is, and what side of you it is

the main camera in our game, also has an audio source
* which is the music
* we set it to `Play On Awake` and `Loop`
* and we don't even need to code anything for that

for the Helicopter explosion sound,
the sound is actually put somewhere else other than the Helicopter object itself
* because we want to play the sound at the same time when we destroy the Helicopter object
* which would also destroy its audio source if we put the sound there
___

# Asset Store

there are tons of things in the Asset Store
* some are also free

we can pay like $10 or $20, then get like 25% of the game done, like right away
* so it's hugely valuable to look at the Asset Store, and think about how we can save our time
* quicker and closer to ship a game to the market
___
