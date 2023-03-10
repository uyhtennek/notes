[Setting Up Your Scripting Environment](https://docs.unity3d.com/2022.2/Documentation/Manual/ScriptingSettingUp.html)
* we cover visual studio only here because it's the most optimal for Unity or game development generally
___

# IDE

Unity supports the following IDEs
* Visual Studio
* Visual Studio Code
* JetBrains Rider

set in menu: Unity > Preferences > External Tools > External Script Editor
___

## Visual Studio
the default IDE on WIndows and macOS

default install location: Program Files > Microsoft Visual Studio > 20XX > Professional
executable can be found at: Common7 > IDE > devenv.exe

in Unity, it needs the Visual Studio Editor package
* Windows > Package Manager > Visual Studio Editor

in Visual Studio, it's recommended to check for update
* Help > Check for Updates
___

# VS productivity features
to open VS in Unity, Assets > Open C# Project

**Unity documentation access**
* highlight or place the cursor over the Unity API, then press **Ctrl + Alt + M**, **Ctrl + H**
* **Help > Unity API Reference**

**Intellisense for Unity API Messages**
= auto-complete for Unity API
* need to take place in a class that derives from `MonoBehaviour`
* **Tab** or **Enter** to complete

**Unity MonoBehavior scripting wizard**
1. **Ctrl + Shift + M** to launch the MonoBehavior wizard
2. Select the method you want to add
	* choose a **Framework version** or **API version**
	* by default, they're inserted at the position of the cursor. alternatively we can choose to insert them after any method that's already implemented in the class
3. mark the **Generate method comments** checkbox if you want
	* these comments are meant to help you understand when the method is called and what its general responsibilities are
4. Choose **OK** to exit the wizard and insert the methods

**Unity Project Explorer**
It shows all of the Unity project files and directories in the same way that the Unity Editor does
* it's different than navigating the project with the normal Visual Studio Solution Explorer
	* which organizes them into projects and a solution generated by Visual Studio
* on the Visual Studio menu, **View > Unity Project Explorer**
	* shortcut: **Alt + Shift + E**


## Unity debugging

1. Connect Visual Studio to Unity by clicking the **Attach to Unity** button
	* keyboard shortcut **F5**
2. Switch to Unity and click the **Play** button to run the game
3. when any breakpoints is encountered, Unity will pause execution of the game and bring up the line of code where the game hit the breakpoint

* **Stop** button, or **Shift + F5** to stop debugging
* for added convenience, we can choose **Attach to Unity and Play mode**
	* so that it will automatically switches to the Unity editor and runs the game on clicking the button or pressing **F5**
	* then **Stop** button will also stop the Unity Editor

**Debug Unity player builds**
* we need to build a development build
	* **File > Build Settings**, mark **Development Build** and **Script Debugging**

**Select a Unity instance to attach the debugger to**
* **Debug > Attach Unity Debugger**
* the dialog displays some information about each Unity instance that we can connect to
	* **Project** - the name of the Unity project that the instance of Unity is running
	* **Machine** - the name of the computer that the instance of Unity is running on
	* **Type** - Editor or Player
		* Player means stand-alone player
	* **Port** - the port number of the UDP socket that this instance of Unity is communicating over

> [!important] Firewall
> Since it's using UDP, we may need to set the firewall to allow it, so that Visual Studio Tools for Unity and Unity can communicate.

**Selecting a Unity instance that doesn't appear in the list**
* if you didn't see the Unity Player listed in the dialog, you can use the **Input IP** button
	* enter the IP address and port of the running Unity Player to connect the debugger
* enable **Tools > Options > Tools for Unity > General > Use saved debug targets**, so that we don't need to enter the IP address and port again

Visual Studio will show saved debug targets as an option in Attach to Unity button.
![visual-studio-tools-unity-saved-target.png (412??167) (microsoft.com)](https://learn.microsoft.com/en-us/visualstudio/gamedev/unity/media/vs/visual-studio-tools-unity-saved-target.png)


## Debug a DLL
Many Unity developers are writing code components as external DLLs so that the functionality they develop can be easily shared with other projects.

> [!warning] native code DLL
> Visual Studio Tools for Unity only supports managed DLLs. It does not support debugging of native code DLLs, such as those written in C++.

> [!warning] third-party code
> We can only debug DLLs that we have the source code, like if we're developing or re-using our own first-party code or we have the source code to a third-party library. We cannot debug a DLL that we do not have the source code.

1. Add your existing DLL project to the Visual Studio solution generated by Visual Studio Tools for Unity.
	* Less commonly, you might be starting a new managed DLL project and if that's the case, add the new managed DLL project to the Visual Studio solution instead.
		* right click solution > **Add > Existing Item...**
2. Reference the correct Unity framework profile in the DLL project.
	* in the DLL project's properties, set the **Target framework** property to the Unity framework version you're using
....
___

# Debug C# code in Unity

managed code debugging in Unity works on all platforms except WebGL
* it works with both the Mono and IL2CPP scripting backends

1. set the Editor's Code Optimization mode to **Debug Mode**
![debug-mode.png (510??107) (unity3d.com)](https://docs.unity3d.com/2022.2/Documentation/uploads/Main/debug-mode.png)

> [!info] Code Optimization setting
> **Debug Mode**: allow attaching external debugger software, but gives slower C# performance
> **Release Mode**: can't attach any external debugger, but give faster C# performance
> 
> To change which mode the Unity Editor starts up in, go to **Edit > Preferences > General > Code Optimization On Startup**
> ![code-op-startup.png (827??420) (unity3d.com)](https://docs.unity3d.com/2022.2/Documentation/uploads/Main/code-op-startup.png)

2. [[#Unity debugging|attach the code editor to the Unity Editor]]
3. after that, return to the Unity Editor and enter Play Mode
___

## Debug in the Unity Player

1. **File > Build Settings**
2. check **Development Build** and **Script Debugging**
	* we could also enable **Wait For Managed Debugger** to make the Player wait for a debugger to be attached before the Player executes any script code
3. **Build And Run**

4. [[#Unity debugging|attach the code editor to the Unity Player]]
	* make sure you sleect the correct instance of the Unity
___

### Debug Android devices

connect to the device using USB or TCP
then attach the code editor to the Unity Player like normal

> [!info] Android on Chrome OS
> Unity can't automatically discover Chrome OS devices. We need to connect to the device through [Android Debug Bridge (adb)](https://developer.android.com/studio/command-line/adb) to initiate a connection.

___

### Debug iOS devices

connect to the device using TCP
then attach the code editor to the Unity Player like normal
* iOS does not support debugging over USB

* ensure that the device only has one active network interface
	* Wi-Fi recommended, turn off cellular data
* and ensure that there is no firewall between the IDE and the device blocking the TCP port
___

## Troubleshoot

* ensure you attach the debugger to the correct Unity instance

* verify the network connection to the Unity instance
	* the code editor uses the same mechanism to locate a Unity instance to debug as the Unity Profiler, so we can try to attach the Unity Profiler to that instance
	* to see if it's the connection or the firewall problem

* ensure the device only has one active network interface
	* to properly connect the debugger to the Unity instance using TCP, the IDE needs to make a network connection to the correct interface on the device
		* eg. if you plan to debug over Wi-Fi, put the device in airplane mode to disable all other interfaces, then enable Wi-Fi
	* ....

* check the firewall settings
	* The Unity instance communicates with the code editor using a TCP connection. Normally it uses an arbitrarily chosen port and the code editor should detect it automatically.
	* If that doesn't work, use a network analysis tool to determine which ports might be blocked either on the machine running the code editor or the machine running the Unity instance.
		* find the ports then make sure the firewall allows access to the port on both machines

* verify the managed debugging information is available
	* if the debugger attaches to the Unity instance but breakpoints don't load, the debugger might not be able to locate the managed debugging information for the code
		* which are some files named .pdb
	* when we enable the correct preferences and build options, Unity generates this debugging information automatically
	* ....

* prevent the device from locking
	* screen locks cause the debugger to disconnect and prevent it from re-connecting
	* don't lock the screen during managed code debugging
	* if the screen locks, restart the application on the device so the debugger can connect again
___

# Stack trace logging

Unity can provide stack trace information for both managed and unmanaged code
* **Managed code**: managed DLLs or C# scripts running in Unity
	* eg. scripts that ship with Unity, scripts that we write, third-party scripts included with an Asset store ploug-in, or any other C# script that runs in the engine
* **Unmanaged code**: native Unity engine code, or code from a native plugin running directly on our machine or on a target build platform
	* usually compiled from C or C++ code
	* we can only access it if we have the original source code of the native binary
	* typically we will use stack trace for unmanged code only if we need to determine whether an error is caused by our code or the engine code, and which part of the engine code

Unity offers three stack trace options
* **None**: Unity doesn't output stack trace information
* **ScriptOnly**: Unity outputs stack trace information only for managed code.
	* The default option.
* **Full**: Unity outputs stack trace information for both managed and unmanaged code

> [!tip] best practices for stack traces
> Resolving a stack trace, especially a full stack trace, is a resource-intensive operation,
> so use stack traces only to debug, and limit the type of messages that display a stack trace.

Editor outputs stack traces to the Console.
Built applications outputs stack traces to [the application's log file](https://docs.unity3d.com/2022.2/Documentation/Manual/LogFiles.html)
___

## Setting the stack trace type

> [!important] it's a build setting
> the stack trace option is a build setting and affects the built player
> not a view preference in the Editor

we have a few options for setting the stack trace type:
* we can use the scripting API - `Application.SetStackTraceLogType`
	* can be used before build or while it's running
* use the Console
	* Console menu button > **Stack Trace Logging > All** or **MESSAGE TYPE**
* **File > Build Settings > Player Settings > Other Settings > Stack Trace**
___
