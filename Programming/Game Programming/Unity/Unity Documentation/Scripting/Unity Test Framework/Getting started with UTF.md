[Getting started with Unity Test Framework | Test Framework | 1.3.3](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/manual/getting-started.html)
to open the **Test Runner** window, **Window > General > Test Runner**
___

# Edit Mode vs. Play Mode tests

**Edit Mode** tests, aka Editor tests
* only run in the Unity Editor and have access to the Editor code in addition to the game code
* can test any of our Editor extensions using the `UnityTest` attribute
	* the test code runs in the `EditorApplication.update` callback loop
* we can control entering and exiting Play Mode from our Edit Mode test
	* so we can make changes before entering Play Mode

**Play Mode** tests can take place in a standalone build or inside the Editor
* allow us to exercise our game code
* the tests run as coroutines if marked with the `UnityTest` attribute


## Conditions

* Both **Edit Mode** tests and **Play Mode** tests need an assembly definition with reference to *nunit.framework.dll*

for **Edit Mode** tests
* the assembly definition needs to have only the Editor as a target platform
* it used to be necessary to put tests in the project's Editor folder, but this is no longer required

for **Play Mode** tests
* the test scripts need to be located in a folder with the .asmdef file
* the test assembly should reference an assembly within the code that we need to test


## Recommendations

* Use the NUnit `Test` attribute instead of the `UnityTest` attribute
	* unless it needs to yield special instructions in Edit Mode
	* or if it needs to skip a frame or wait for a certain amount of time in Play Mode
___
# 1. Create a new test assembly

What are `TestAssemblies`?
* Unity Test Framework looks for a test inside any seembly that references NUnit
* **Play Mode** and **Edit Mode** tests need to be in separate assemblies

By default the **Test Runner** window has the **EditMode** tab enabled by default, so we create the Edit Mode tests first
* click the **Create EditMode Test Assembly Folder** button
* it'll create a folder
	* change its name if preferred
	* the .asmdef file inside of it follows its name

the .asmdef file should have
* references to **nunit.framework.dll, UnityEngine.TestRunner, UnityEditor.TestRunner** assemblies
	* the **UnityEditor.TestRunner** reference is only available for Edit Mode tests
* as well as **Editor** preselected as a target platform
___

# 2. Create a test

1. after created the *Test* assembly folder, select it in the **Project** window
2. click the **Create Test Script in current folder** button in the **Test Runner** window
3. it creates a *NewTestScript.cs* file in the *Tests* folder
	* change the name if necessary

The test script has two sample tests and we can see it from the Test Runner window
![test-templates.png (512×229) (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/manual/images/test-templates.png)

We can also create test scripts by navigating to **Assets > Create > Testing > C# Test Script**
___

## Create additional tests
create another set of tests

1. Create a new test assembly folder
* In the **Project** window, **Assets > Create > Testing > Test Assembly Folder**
* remember to change its name because assembly definition names must be unique
	* note that changing the file name of the assembly definition file does not affect the value of the **Name** property in the file

2. Select the new folder in the **Project** window

3. Create a new test script in the folder
* **Assets > Create > Testing > C# Test Script**

By default **Any Platform** is preselected as the target platform for the new assembly
* so it's a PlayMode test and will appear as a PlayMode test in the TestRunner window
* to change it to an EditMode test, select **Editor** only under **Platforms** in the **Inspector**

New assemblies created should automatically include references to `UnityEngine.TestRunner` and `UnityEditor.TestRunner`. If these references are missing, add them back.
![assembly-definition-references.png (862×266) (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/manual/images/assembly-definition-references.png)


## Filters
we can view/run a sub-set of tests, by filtering tests

![test-templates.png (512×229) (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/manual/images/test-templates.png)
* Type in the search box in the top left
* Click a test class or fixture, such as **NewTestScript** in the image above
* Click one of the test result icon buttons in the top right
___

# 3. Run a test

* double-click on the test or test fixture name in the **Test Runner** window
* click **Run All** or **Run Selected** button
* right click on any item in the test tree to open up a context menu and choose **Run**

Then the test result will be shown by
* changing the test status icon
* and a counter in the top right corner updated
![editmode-run-test.png (512×245) (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/manual/images/editmode-run-test.png)
___

# 4. Create a Play Mode test

....
___

# 5. Run Play Mode tests

If we run a **Play Mode** test in the same way as an Editor test, it runs inside the Unity Editor

We can also run Play Mode tests on specific platforms, by click **Run all in the player** to build and run the tests on the currently active target platform
* then it will build the application
* then run the tests and finally displays the test result
![playmode-results-standalone.png (512×403) (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/manual/images/playmode-results-standalone.png)

Once the test results are reported back to the Editor UI, it shutdowns the application
* make sure both the Player and Editor are on the same network
	* if Unity cannot instantiate the connection, you can see the tests succeed in the running application
* some platforms don't support `Application.Quit` so it will continue running after reporting the test results
___
