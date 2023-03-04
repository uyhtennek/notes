# Lighting Setting

We can go to Window > Lighting > Settings,
to see all the global Unity lighting settings for the scene
* we can set things like Skybox, Environment Lighting, Fog, Bake Lighting, etc.


## Environment Lighting

In our game, actually there is no lighting except Environment Lighting
which is just a nasty dark green color in our case

It applies an uniform, ambience, light
* kinda like a directional light as they are both global to all the objects in the scene, except it doesn't have a direction
* it is applied to everything in the scene, at a given intensity


## Fog

Just check a button and choose a color, maybe set a density too, then we made Fog in our scene.
* if it's a higher density fog, the fog color applied to the objects are deeper
	* especially for things that are closer to us


> [!...]
> We're bringing up these topic because they are making our level hard to see
> so we disabled fog, and added a directional light to the scene

___

# Maze Generation

it's a 2D array, since our maze is just one level, we just care about where the walls are
* even still, we may divide different levels as separate 2D arrays instead of a 3D array
* where the walls are, is just represented by the value in each x and y
	* in our case they are all represented using booleans, so either have a wall or no wall
	* didn't have different types of wall

say we populated a 2D array now, an empty one, just all zeros
what is the next step? Start with 4 walls and some corridors.
* actually we start with a 2D array with everything is `true`
* we don't touch the "outer-most" row and column, leaving the center as our working area

then we will start carving our way through the maze, turning some ones to zeros
* we start with a random position
* then move either left or right or up or down, move either in x or y, but not both at once

* so the algorithm, in every step, just randomly choose, "should I move x or should I move y? then shoud I move positive or negative?"
* in the code, we also teleport the character to the first empty space too

* we also have a counter, keeping track of how many blocks that we cleared
* so that the algorithm stops at some point before clearing all the walls

now the algorithm just going to move randomly where ever it happy
but that doesn't necessarily going to create a nature or interesting maze right?
* like it doesn't guarantee to generate a corridor or hallway
* also it is likely to create a weirdly large room

* we could just randomly choose a row or column and clear all its blocks
	* however, very long hallways like that may not be interesting as well

what we end up doing is, when we choose the next direction to move or carve
We also choose how many times to move
* so we can carve in a direction several blocks at once
* creating corridors of random length

lastly we need to clamp the times to move in a way
so that we don't move outside of the boundary

> [!info]
> Surely this is just a simple example and simple algorithm. Mazes in like a crossword puzzle book or a maze book, are more complicated and generated differently.

## Other methods
* [Maze, a Unity C# Tutorial (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/maze/)
* [Rooms and Mazes: A Procedural Dungeon Generator – journal.stuffwithstuff.com](http://journal.stuffwithstuff.com/2014/12/21/rooms-and-mazes/)
* There are assets like those in Asset Store, either free or paid
	* much of them are also very customizable so we can tailor the generator to fit our own game


## Code Detail

```c#
// file: LevelGenerator.cs
public GameObject floorPrefab;
public GameObject wallPrefab;
public GameObject ceilingPrefab;

public GameObject characterController;

// we make the generated block as children of the parent object
// because if we haven't do so, they will just fill up our Hierarchy very messily
public GameObject floorParent;
public GameObject wallsParent;
...
void Start() {
	mapData = GenerateMazeData();
	
	// x and y axes are the ground
	for (int z = 0; z < mazeSize; z++) {      // z is front and back axis
		for (int x = 0; x < mazeSize; x++) {  // x is left and right axis
			if (mapData[z, x]) {
				// if the value in this cell is true, generate a wall
				// walls are 3 blocks high
				CreateChildPrefab(wallPrefab, wallsParent, x, 1, z);
				CreateChildPrefab(wallPrefab, wallsParent, x, 2, z);
				CreateChildPrefab(wallPrefab, wallsParent, x, 3, z);
			} else if (!characterPlaced) {
				// so this cell is empty, doesn't have a wall
				// we place the character to the first empty cell
				characterController.transform.SetPositionAndRotation(
					new Vector3(x, 1, z), Quaternion.identity
				);
				characterPlaced = true;
			}
			
			// always have the floor
			CreateChildPrefab(floorPrefab, floor, x, 0, z);
			
			if (generateRoof) {
				// only generate the roof if we want to
				CreateChildPrefab(ceilingPrefab, wallsParent, x, 4, z);
			}
		}
	}
}

bool[,] GenerateMazeData() {
	bool[,] data = new bool[mazeSize, mazeSize]
	...
	return data;
}

void CreateChildPrefab(GameObject prefab, GameObject parent, int x, int y, int z) {
	var myPrefab = Instantiate(prefab, new Vector3(x, y, z), Quaternion.identity);
	// making `myPrefab` a child of `parent`
	myPrefab.transform.parent = parent.transform;
}
```

Notice in C#, 2D arrays are declared with `[,]`
* it's different than in a lot of other languages, it has its own set of syntax
* the comma `,` is to designate that it takes 2 arguments to index
* if we want a 3D, 4D, 5D arrays and so forth, just add more commas to it

> [!tip]
> We're generating a wall as 3 cubes, but surely it's not ideal.
> 
> Even in Minecraft we'd think everything is a cube, or block, seperated. But actually the world is one massively geometry instead of many little blocks, and then it dynamically figures out where we're hitting and removes and adds blocks. So that it's optimized.
> 
> We can do this in Unity too. We can dynamically create meshes and vertices and stuff in Unity.

___

# Character Controller

Unity has a set of built-in standard asset
* Assets > Import Package
	* we used the Prototyping package for the pick up and
	* the Characters package for the FPSController
		* we can find it in the Project window after we imported it
		* Assets > Standard Assets > Characters > FirstPerson > Prefabs > FPSController
		* to use it, just drag it into our scene

The FPSController effectively, is just a capsule collider
* which is of a physics type - kinematic
	* but with gravity applied to it
* and it has a camera placed towards the top sort of like where the head is
* there are some programming done already, to allow us to control it with the keys and the mouse
	* we can look at the scripts and change them too, since they come with the package

There are some amount of customizable allowed for free
* we can set its walk speed, run speed, jump speed, mouse look sensitivity, etc.
* there is a thing called *FOV Kick*
	* which means when we're sprinting, by pressing Shift, it'll increase the FOV a little bit
		* to make it look as if you're kinda claustrophobic, the view is more narrow
	* we can set its increase curve too
* there is another thing called *Head Bob*
	* which means, as we walk, should the camera go up and down
	* again, we can set its Bobcurve
* also footstep sounds and jump sound and land sound

It's neat, but it doesn't really give much more than that
* if we want to do a FPS where we can have gun or weapon or something, we need to program some more things
* for just basic navigation of a 3D scene, it's a great foundation


## More things

In the same package, there are
* a third-person character controller
	* doesn't come with a camera though, it supposes us to place the camera ourselves
	* camera programming is another big topic
* a character model
* an character controlled by AI
	* so we can test AI in our scene
___
