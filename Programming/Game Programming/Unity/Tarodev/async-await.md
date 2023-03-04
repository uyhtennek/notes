[Unity async / await: Coroutine's Hot Sister [C# & Unity]](https://www.youtube.com/watch?v=WY-mk-ZGAq8)
___

# Coroutine to Async

```c#
public IEnumerator RotateForSeconds(float duration)
{
	var end = Time.time + direction;
	while (Time.time < end)
	{
		transform.Rotate(new Vector3(1, 1) * Time.deltaTime * 150);
		yield return null;
	}
}
// some where else called
// StartCoroutine(RotateForSeconds(1));
```

```c#
public async void RotateForSeconds(float duration)
{
	var end = Time.time + duration;
	while (Time.time < end)
	{
		transform.Rotate(new Vector3(1, 1) * Time.deltaTime * 150);
		await Task.Yield(); // almost the same as `yield return null`
	}
}
// some where else called
// RotateForSeconds(1);
```
___

# Sequential function calls
call functions, one **after** one

async functions can run sequentially
* coroutine can also do, but the code will be messy

```c#
public async Task RotateForSeconds(float duration)
{
	var end = Time.time + duration
	while (Time.time < end)
	{
		transform.Rotate(new Vector3(1, 1) * Time.deltaTime * 150);
		await Task.Yield();
	}
	// note we don't need to return anything, C# automated this returning part
}
```

`Task`
* similar to a `Promise` in JavaScript
* it allows us to monitor if the function is done, and how long it has elapsed.
	* thus we can cancel it as well

```c#
public async void BeginTest() {
	await RotateForSeconds(1); // wait for the async function to finish
	// then go to next line
	...
}
```
___

# Synchronous function calls
call functions, all **at once**

```c#
public async void BeginTest() {
	var tasks = new Task[3];
	
	tasks[0] = RotateForSeconds(1);
	tasks[1] = RotateForSeconds(2);
	tasks[2] = RotateForSeconds(3);
	
	await Task.WhenAll(tasks); // wait for all 3 tasks to finish
	// then go to the next line
	...
}
```
___

# Return value

```c#
async Task<int> GetRandomNumber() {
	var randomNumber = Random.Range(100, 300);
	await Task.Delay(randomNumber);  // Task(C#) uses milliseconds
	return randomNumber;
}
```

```c#
public async void BeginTest() {
	var randomNumber = await GetRandomNumber();
	print(randomNumber);
}
// or
public void BeginTest() {
	var randomNumber = GetRandomNumber().GetAwaiter().GetResult();
	// or use `GetRandomNumber().Result`
	print(randomNumber);
}
```

`var randomNumber = await GetRandomNumber();`
* if we didn't put the `await` here, it would return the `Task` object, instead of its return value

`GetRandomNumber().Result`
* if we used a try clause in `GetRandomNumber()`, and threw an catch error, we would get a really generic error as the `Result`
* so it is always better to use `GetRandomNumber().GetAwaiter().GetResult()`
___

# WebGL caveat

`async` doesn't work for WebGL
need to use Coroutine....

9 Oct 2021
___
