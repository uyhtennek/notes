[QA your code: The new Unity Test Framework - Unite Copenhagen](https://www.youtube.com/watch?v=wTiF2D0_vKA)
New Unity Test Framework, 2019
___

# Unity Test Framework
= new version of TestRunner

it's a package now
* we have the source code locally in our Unity Editor

it has the TestRunner API now, so we can
* launch tests programmatically
	* `void Execute(ExecutionSettings executionSettings);`
* retrieve a list of tests
	* both in Edit mode or Play mode
	* can just retrieve without running
	* `void RetrieveTestList(TestMode testMode, Action<ITestAdaptor> callback);`
* register/unregister callbacks
	* `void RegisterCallbacks<T>(T testCallbacks, int priority = 0) where T : ICallbacks;`
	* `void UnregisterCallbacks<T>(T testCallbacks) where T : ICallbacks;`

.....
___
