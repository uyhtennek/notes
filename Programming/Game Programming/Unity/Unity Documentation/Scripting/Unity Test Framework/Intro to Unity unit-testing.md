[Introduction To Unity Unit Testing | Kodeco](https://www.kodeco.com/9454-introduction-to-unity-unit-testing)
* what is unity-testing
* the value it offers
* pros and cons
* how it works in Unity using the Test Runner
* writing and running unit tests that pass
___

# What is a Unit Test?

Ideally, a unit test is for testing a single "unit" of code.
* keep in mind that a unit test should be testing exactly one 'thing' at a time
* a unit test is for validating that a small, logical, snippet of code performs exactly as we expect it to in a specific scenario

eg. we've written a method that allows the user to input a name
* no numbers allowed in the name
* the name can only be ten characters or less
```c#
public string name = "";
public void UpdateNameWithCharacter(char: character)
{
	// call on every keystroke
	
	// 1
	if (!Char.IsLetter(char)) {
		return;
	}
	
	// 2
	if (name.Length > 10) {
		return;
	}
	
	// 3
	name += character;
}
```
___

# Test Suite

we can think as there are 3 units in this method
and we can test each of them

so we need 3 test methods
* `UpdateNameDoesntAllowNonLettersToBeAddedToName`
* `UpdateNameDoesntAllowCharacterAddingToNameIfNameIsTenOrMoreCharactersInLength`
* `UpdateNameAllowsLettersToBeAddedToName`

and we can think of these unit tests are under a **test suite**
* a test suite houses all unit tests related to a logical grouping of functionality
	* eg. the combat unit tests, the update name unit tests, etc.
* if any individual test in a test suite fails, the entire test suite fails
___

# Test Runner
open up Unity Test Runner: **Window > General > Test Runner**

Test Runner is the unit testing feature provided by Unity
* it utilizes the **NUnit** framework

To create a Play Mode test
1. In the **Project** window, select a directory you would like to create the test assembly folder
2. click **Create PlayMode Test Assembly Folder** in the **TestRunner** window
3. input a name for the folder and press Enter

**PlayMode** tests will run while in Play mode, as if we were playing the game in real time
**EditMode** tests will run outside of Play mode, suitable for testing things like custom Inspector behaviors
___

## Creating Test Suite

since a unit test is a method, it needs to be in a class file in order to run
* that class file is called a test suite
* create that file by clicking the **Create Test Script in current folder** button

A test suite is where we logically divide our tests
* eg. a test suite for physics and a separate one for combat
___

## Setting up Test Suite

we can see there is a **.asmdef** file in the folder
* it's an *assembly definition file*
* it points Unity to where the test file dependencies are
	* because our production code is kept separate from our test code

so if you run into a situation where Unity can't find your test files or tests, make sure there's an assembly definition file that includes the test suite
* actually we need to set up this assembly definition file by ourselve

1. **Create > Assembly Definition**
* can create in the folder where we normaly put our scripts in
* name it, eg. GameAssembly

2. In our test assembly definition file, under **Assembly Definition References** in the **Inspector**, add the newly created GameAssembly file

3. Hit **Apply**

without these steps, we can't reference the game class files inside the unit test files
___

## Writing Unit Tests

open up the **TestSuite** script we previously created
* clear all the boilerplate code
```c#
using System.Collections;
using NUnit.Framework;
using UnityEngine;
using UnityEngine.TestTools;

public class TestSuite
{

}
```

so what tests should we write?
* for this tutorial, we will only concern about a few key areas around hit detection and core game mechanics
* for a production level product, it's worth the time to consider all the possible edge cases for all areas we need to test

eg. write a test to make sure the asteroids actually move down
```c#
private Game game;

[UnityTest]
public IEnumerator AsteroidsMoveDown()
{
	GameObject gameGameObject = MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
	game = gameGameObject.GetComponent<Game>();
	
	GameObject asteroid = game.GetSpawner().SpawnAsteroid();
	float initialYPos = asteroid.transform.position.y;
	
	yield return new WaitForSeconds(0.1f);
	Assert.Less(asteroid.transform.position.y, initialYPos);
	Object.Destroy(game.gameObject);
}
```

`[UnityTest]`
* this is an **attribute**, which define special compiler behaviors
* it tells the Unity compiler that this is a unit test

`gameGameObject`, `game`
* in this case, everything is inside the script `Game` and it's a `MonoBehavior`, so `game` actually is the entire game
	* and it's stored in a prefab - `"Prefabs/Game"`
* in a production environment, this is likely not the case, so we need to take care to recreate all the objects needed in the scene

`initialYPos`
* keep track of where the asteroid is when it spawns
* the Asteroid component has a `Move` method on it so it takes care its own movement

`yield return new WaitForSeconds(0.1f);`
* all Unity unit tests are coroutines so we need to add a yield return
* we can put `yield return null;` here if we like

`Assert.Less(asteroid.transform.position.y, initialYPos);`
* this is the **assertion** step
* passing or failing the test is determined by this line

`Object.Destroy(game.gameObject);`
* clean up the testing environment
* because it might causes other tests to fail
___

## Running Unit Tests

save the script and open up Test Runner
expand all the arrows to see the test we wrote

if a test has not yet been run, it shows a gray circle
if it's run and passed, it'll show a green tick
if not, it'll show a red X

run the test by click the **RunAll** button
* it'll create a temporary scene and enter Play Mode to run the test
___

# Integration Tests

*Integration tests* are tests that validate how "modules" of code work together.
* it's more than just a unit
* it's designed to test how the software works in actual production

![IntegrationTests-1.png (511×533) (raywenderlich.com)](https://koenig-media.raywenderlich.com/uploads/2018/12/IntegrationTests-1.png)

eg. if we've made a combat game where the player kills monsters, we might want to make an integration test to make sure that when a player kills 100 units, an achievement unlocks
* the test involves several modules of our code
	* eg. the physics engine for handling hit detection, the unit managers for tracking unit health and process damage, the event tracker, ...., and finally the achievement manager

we won't cover integration test in this tutorial though
___

# Test phases

now say we wrote another test
```c#
[UnityTest]
public IEnumerator GameOverOccursOnAsteroidCollision()
{
    GameObject gameGameObject = 
       MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
    Game game = gameGameObject.GetComponent<Game>();
    
    GameObject asteroid = game.GetSpawner().SpawnAsteroid();
    asteroid.transform.position = game.GetShip().transform.position;
    
    yield return new WaitForSeconds(0.1f);
    Assert.True(game.isGameOver);
    Object.Destroy(game.gameObject);
}
```

we can see there are some code repeated among this new test and the previous test
```c#
GameObject gameGameObject = 
	MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
Game game = gameGameObject.GetComponent<Game>();

// and
Object.Destroy(game.gameObject);
```

There are actually two phases when it comes to running a unit test:
* the **Setup** phase
	* before running the unit test
* the **Tear Down** phase
	* after running the unit test

so we can make life easier by moving things into the setup and tear down phases
```c#
public class TestSuite
{
	private Game game;
	
	[SetUp]
	public void Setup() {
		GameObject gameGameObject = 
			MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
		Game game = gameGameObject.GetComponent<Game>();
	}
	
	[TearDown]
	public void TearDown() {
		Object.Destroy(game.gameObject);
	}
	
	...
}
```
* and remember to remove the duplicated code in the test methods
___

# Checking for null

eg. we want to make sure that a laser will destroy an asteroid if it hits it
```c#
[UnityTest]
public IEnumerator LaserDestroysAsteroid()
{
	GameObject asteroid = game.GetSpawner().SpawnAsteroid();
	asteroid.transform.position = Vector3.zero;
	
	GameObject laser = game.GetShip().SpawnLaser();
	laser.transform.position = Vector3.zero;
	
	yield return new WaitForSeconds(0.1f);
	UnityEngine.Assertions.Assert.IsNull(asteroid);
}
```

`UnityEngine.Assertions.Assert.IsNull(asteroid);`
* Unity has a special `Null` class which is different from a "normal" `Null` class
* when check for nulls in Unity, we must explicitly use the `UnityEngine.Assertions.Assert`, not the NUnit `Assert`
___

# Test Driven Development
shorts as *TDD*

with TDD, we actually write tests before we write our application logic
* we make tests first, ensure they fail
* then write code designed to get the test to pass
	* it ensures we're writting code in a testable way

> [!info] test both public and private methods?
> Some people believe that private methods should only be tested through the public methods that use them.
> Because testing private methods can be problematic and requires special frameworks or using reflection tools to check things.
> But the "unit" of code we need to test can get quite large if we test only public methods.

___

# Pros and Cons

Why we need unit testing
* provide confidence that a method behaves as expected
* serve as documentation for new people learning the code base
	* unit tests make for great teaching
* force us to write code in a testable way
* help isolating bugs faster and fix the quicker
* prevent future updates from adding new bugs to old working code
	* known as *regression bugs*

Why we don't like unit testing
* extra time or budget needed
* writing tests can take longer than writing the code itself
* bad or inaccurate tests create false confidence
* need many knowledge to implement correctly
* many code base might not be easily testable
* some frameworks don't easily allow private method testing
* if tests are too fragile, maintenance can take a lot of time
* unit tests don't catch integration errors
* UI is hard to test
* inexperienced developers might waste time testing the wrong things
* sometimes testing things with runtime dependencies can be very hard
___
