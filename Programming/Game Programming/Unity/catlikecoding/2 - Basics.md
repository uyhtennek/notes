[Game Objects and Scripts (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/basics/game-objects-and-scripts/)
[Building a Graph (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/basics/building-a-graph/)
___

# Unity versions

Unity has two parallel release schedules
* the most stable and safe are the LTS releases
	* stands for *long term support*, which is 2 years in Unity's case

eg. `Unity 2020.3.6`
* the third portion of the version number indicates the patch release
	* contain bug fixes and rarely new functionality
* a further f1 suffix indicates an official final release

The highest Unity version is the of the development branch
* introduces new features and possibly removes old functionality
* not as reliable as LTS versions, and only remain supported for a few months each
___

# Packages

There are quite a few default packages
* we can hide them, purely for reduce visual clutter in 
	* there is a toggle at the top right of the project window that looks like an eye
	* they are still part of the project after we hid them

We can control which packages are included in our project, via package manager
* opened via *Window / Package Manager* menu item

Packages add extra functionality to Unity
* eg. *Visual Studio Editor* adds integration for the Visual Studio editor
* we don't need any of them except *Visual Studio Editor* or *Visual Studio Code Editor* if we use VS or VS Code

* after that, there are not only *Visual Studio Editor* left, but also *Custom NUnit* and *Test Framework*
	* because *Visual Studio Editor* depends on them
* to make dependencies and implicitly imported packages visible in the package manager
	* via *Edit / Project Settings > Package Manager*, enable *Show Dependencies* under *Advanced Settings*
___

# Color Space

Nowadays rendering is usually done in linear color space, but Unity still configured to use gamma color space by default
* actually this is not the case any more, at least in the Unity version 2021

* For best visual results, *Edit / Project Settings > Player > Other Settings*, looks for *Rendering* section and set *Color Space* to *Linear*
	* Unity will show a warning saying that this might take a long time
	* don't need to worry for a nearly-empty project, so Confirm

> [!faq]- When to use gamma color space?
> Only when you're targeting old hardware or old graphics APIs. OpenGL ES 2.0 and WebGL 1.0 don't support linear space.
> Besides, gamma can be faster than linear on old mobile devices.

___

# Sample Scene

By default, the project window uses a two-column layout
* we can switch to a one-column layout via its triple-dot configuration menu option

We can select a game object either via the hierarchy window or the scene window.

To navigate the scene window,
* we can use the alt or option key in combination with the cursor to rotate the view
* we can use the arrow keys to move the point of view
* can do zoom by scrolling
* press F key to focus the view on the game object currently selected
* etc.

In the hierarchy window, there are an eye icon and a hand icon next the the object
* they are invisible by default but will appear when we hover the cursor there
* eye icon to hide the game object
	* purely to reduce visual clutter in the scene window
* hand icon to make the game object not selectable via the scene window
___

# Demo 1

the rightmost button in the group of manipulation mode toolbar is for enabling custom editor tools, which we don't have
![manipulation-toolbar.png (820×56) (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/basics/game-objects-and-scripts/building-a-simple-clock/manipulation-toolbar.png)


Unity's primitive 3D objects typically have 3 components other than `Transform`
![cylinder-inspector.png (640×640) (catlikecoding.com)|460](https://catlikecoding.com/unity/tutorials/basics/game-objects-and-scripts/building-a-simple-clock/cylinder-inspector.png)

* `MeshFilter` contains a reference to the built-in mesh, it's a cylinder in this example

* `MeshRenderer` ensures the object's mesh gets rendered
	* it also determines what material is used for rendering, it's the Default-Material in our case

* `CapsuleCollider`, for 3D physics
	* fun fact, Unity represents a cylinder mesh with a capsule collider, because it doesn't have a primitive cylinder collider


Child objects are subject to the transformation of their parent object
* when the parent object changes position, rotation or scale, the child does as well
* as if they are a single entity


What is albedo?
* Albedo is a Latin word which means whiteness.
* It's the color of something when illuminated by white light.


To change an object's material, we can do either
* drag the material asset onto the object in either the scene or hierarchy window
* drag it to the bottom of the inspector window when the game object is selected
* or change *Element 0* of the *Materials* array of the object's `MeshRenderer` component


We can click on the axis cones of the view camera gizmo at the top right of the scene view
* we can also change the axis of the scene grid, via the grid toolbar button


We can duplicate a game object, either via
* *Edit / Duplicate*
	* also can see the keyboard shortcut here
* the object's context menu in the hierarchy window


> [!info] math for a clock
> Each hour covers a $30\degree$ clockwise rotation along the Z axis. Since Unity's rotation is counterclockwise, we use negative rotations.
> 
> We can find the position for hour 1 via trigonometry. The sine of $30\degree$ is $\frac{1}{2}$ and its cosine is $\frac{\sqrt{3}}{2}$. We scale those by the distance at which we position the hours indicator from the center, which is 4.
> 
> Therefore, the position for the indicator of hour 1 is:
> $x = 4 \times \frac{1}{2}$ | $y = 4 \times \frac{\sqrt{3}}{2}$
> $x = 2$ | $y = 2\sqrt{3} \approx 3.464$
> 
> Then for hour 2 the rotation is $60\degree$, so we can simply swap the sine and cosine
> $x \approx 3.464$ | $y = 2$
> 
> <center><img src="https://catlikecoding.com/unity/tutorials/basics/game-objects-and-scripts/building-a-simple-clock/hour-indicators-1-2.png"></center>


Transform is always done in an object's center
* we actually have to introduce a pivot object and perform transformation on that instead
* make sure the tool handle position mode is set to *Pivot* instead of *Center*
___

# C# 1

`class Clock`
What is a class?
* a blueprint that can be used to create objects
* defines what data these objects contain and what functionality they have
* can also define data and functionality that don't belong to object instances, but to the class itself
	* often used to provide globally-available functionality

`public class Clock`
* because we don't want to restrict which code has access to our `Clock` type, we prefix it with the  `public` access modifier

> [!info] `internal` is the default access modifier
> Without the access modifier, it would be as if we had written `internal class Clock`.
> so `class Clock` is the same as `internal class Clock`

`public class Clock: UnityEngine.MonoBehaviour { }`
To make a class as component in Unity, it must extend Unity's `MonoBehaviour` type, inheriting its data and functionality
* the type `MonoBehaviour` is contained in the namespace `UnityEngine`

> [!info] What does mono-behavior mean?
> behavior: we can program our own components to add custom behavior to game objects
> mono: Unity used the Mono project, a multi-platform implementation of the .NET framework
> 
> Hence, `MonoBehaviour`
> It's an old name that we're stuck with due to backwards-compatibility.

> [!info] What is a namespace?
> It's like a website domain, but for code.
> 
> Like domains can have subdomains, namespaces can have sub-namespaces.
> Except unlike domains, it's written the other way around.
> so we would write `com.unity.forum` instead of `forum.unity.com`
> 
> Namespaces are used to organize code and prevent name clashes.
> 
> The assembly containing the `UnityEngine` code comes with Unity.
> We don't have to go online to fetch it separately.
> 
> The project file used by the code editor should be set up automatically to recognize it, if the appropriate editor integration package is imported.


We can declare that the namespace should be searched automatically to complete type names in the C# file, by adding `using UnityEngine;` at the top of the file


To add our custome component to a game object, either
* drag the script asset onto the object
* drag the sciprt asset to the object's inspector
* via *Add Component* at the bottom of the object's inspector


`[SerializeField] Transform hoursPivot;`
* fields are private by default, meaning they can only be accessed by the code belonging to the same class
* declaring a field as serializable means it should be included in the scene's data when Unity saves the scene
	* under the hood, which is done by putting all data in a sequence, serializing it, and writing it to a file
	* done by attaching a `SerializeField` attribute to it

* typically written in 2 ways
```c#
[SerializeField]
Transform hoursPivot;
// or
[SerializeField] Transform hoursPivot;
```

* we can just make it `public`, but it is generally a bad to make class fields publicly accessible
* the rule of thumb is to only make class contents public, if C# code from other types need access to it, and then prefer methods or properties over fields

> [!tip]
> The less accessible something is, the easier it is to maintain.
> Because there is less code that could directly depend on it.


To make the proper connection between the custom serialized field and the object or component
* we can drag the object from the hierarchy onto the field
* alternatively, use the circular button at the right of the field and search for the object


`void Awake () { }`
* Unity will invoke this method on the component, after it's been created or loaded
	* while in play mode

> [!question] Does `Awake` need to be `public`?
> Doesn't matter.
> 
> `Awake` and a collection of other methods are considered special Unity event methods. Unity engine will find them and invoke them when appropriate, no matter how we declare them.
> This happens from outside the managed .NET environment.


A property is a method that pretends to be a field.
* can be read-only or write-only
* C# convention is to capitalize properties
	* although Unity's code doesn't do this. their properties don't do this.
	* eg. `localRotation`, from the `Tranform` type, is actually a property


Although the rotation of a `Transform` component is defined with Euler angles in degrees per axis, in code we have to do it with a quaternion
* Quaternions are based on complex numbers and are used to represent 3D rotations
* althought it is harder to understand, they have some useful characteristics
	* eg. they don't suffer from gimbal lock


What is a `struct`?
* short for structure
* like a class, it's a blueprint
* the difference is that, whatever it creates is treated as a simple value, like an integer or color, instead of an object


`rotation` versus `localRotation`
* the `localRotation` property represents the rotation described by the `Transform` component in isolation, thus it's the rotation relative to its parent
	* it's the rotation that we see in its inspector
* the `rotation` property represents the final rotation in world space
	* taking the entire object hierarchy into account
* used an incorrect one often produce weird results


We often times will declare a variable, but not initialize it in the code, since we do that in the Editor
* normally compiler can detect empty variable and issue a warning to us
	* because it is unware that we set it up via Unity's inspector
* however, the warning is suppressed by default
	* In *Player / Other Settings / Script Compilation*, there is a *Suppress Common Warning* toggle
	* it suppresses warning about both uninitialized and unused private fields


To enter play mode,
* *Edit / Play*
	* keyboard shortcut is also here
* press the play button at top center of the editor window


Keep in mind that the scene reset when exiting play mode
* any changes made to the scene while in play mode will not persist
* but, changes to assets always persist

We can have multiple scene windows and game windows open while in play mode, if wanted
* although keep in mind that Unity has to render all these windows
* so the more you have open, the slower things get


We can use the `DateTime` struct to access the system time of the device we're running on
* `DateTime` is not a Unity type, it is found in the `System` namespace
	* it's part of the core functionality of the .NET framework
		* .NET framework is what Unity uses to support scripting

`DateTime` has a `Now` property
* that produces a `DateTime` value containing the current system date and time
* also an `Hour` property, that gives us the hours portion of a `DateTime` value

> [!info]- What is a `float`?
> For example $\frac{1}{3}$, just like how we cannot exactly write that number in decimal notation, the computer can't store its all numbers within a finite memory size. The best it can do is write `0.3333333` and then stop at some point.
> 
> Let's say we want to write at most 3 digits after the dot and only one in front of it. So $\frac{1}{3}$ is `0.333`. Now we divide it by 100, then it's forced to write as `0.003`. Therefore, we lost two digits of percision.
> 
> To improve percision, let's add a second piece of information, a separate exponent. Now to represent $\frac{1}{300}$, we would write it as $0.333\times10^{-2}$. Therefore, we don't lose meaningful digits.
> 
> A `float` is such a value stored in 4 bytes, so 32 bits.

> [!info]- What is `const`?
> The `const` keyword indicates that a value will never change and doesn't need to be a field.
> Its value will be computed during compilation and is substituted for all usage of the constant.
> It is only possible for primitive types like numbers.

> [!info]- variable and field
> A variable acts like a field, except that it exists only while a method is being executed. It belongs to the method, not the class.
> 
> It is possible to omit the type declaration, by replacing it with the `var` keyword. But it is only possible when the variable's type can be inferred from what is assigned to it when declared.
> Prefer to only do this when the type is explicitly mentioned in the statement.

> [!info]- What is a frame?
> While in play mode Unity continually renders the scene from the point of view of the main camera. Once rendering is down the result is presented to the display. The display will then show that frame until it gets the next one.
> 
> Before rendering a new frame, everything gets updated. So Unity go through a sequence of update, render, update, render, update, and so on.
> 
> So a single frame is a single update step followed by rendering the scene once. Although in reality it is more complicated than that.


To prevent Unity from invoking the `Update` method of a component
* just disable it, by unchecking the toggle of the component in the inspector


`DateTime` has a `TimeOfDay` property
* It has the `TotalHours`, `TotalMinutes`, `TotalSeconds` properties
	* they give us a `TimeSpan` value


Unity's code only works with single-precision floating point values
* so it only works with `float` but not `double`
* Game engines typically use single-precision floating-point values, and so do GPUs
* because double precision waste memory

* When it will become a problem?
	* If we were working with very large distances or scale differences.
	* then we'll have to apply tricks like teleportation or camera-relative rendering
		* to keep the active area near the world origin
___

# Maths
Mathematics is essential when programming

At its most fundamental level, math is the manipulation of symbols that represent numbers
* solving an equiption boils down to
	* rewriting one set of symbols so it becomes another set of symbols
	* usually shorter

We have a function $f(x) = x + 1$
* we input 3, we get an output 4
* we can say that the function maps 3 to 4
* a shorter way to write this would be as an input-output pair, like $(3,4)$
	* of the form $(x,f(x))$
	* if we would write multiple pairs, it's easier to understand the function when we order the pairs by the input number
		* eg. $(1,2)$ and $(2,3)$ and $(3,4)$ and so on

$f(x) = x + 1$ is easy to understand. $f(x) = (x - 1)^{4} + 5x^{3} - 8x^{2} + 3x$ is harder.
* writing down input-output pairs probably won't give us any clue
* we're going to need many points, close together
	* so we interpret the pairs as two-dimensional coordinates of the form $\begin{bmatrix} x \\ f(x) \end{bmatrix}$
	* it's a 2D vector
		* the top number represents the horizontal coordinate, on the X axis
		* the bottom number represents the vertical coordinate, on the Y axis
	* in other word, $y = f(x)$
* we end up with a line, effectively a graph
	* which can quickly give us an idea of how a function behaves
___

# Prefab

To edit a prefab in the Unity's dedicated scene for editing prefabs
* click the *Open Prefab* button when selected the prefab asset
* click the *Open* button of an instance
* click the right arrow next to an instance in the hierarchy window
* double-click the asset in the project window

The default prefab scene has an uniform dark blue background
* if we open a prefab instance that's part of a scene, then the scene window will display its surroundings depend on the *Context* settings shown at the top of the window
* if we open the prefab asset, then there is no context, so it just give a dark blue background

* prefab scene by default disabled skybox and some other things
	* we can configure this via the scene window's toolbar
	* just like we can for the regular scene window
* the skybox can be toggled via the dropdown menu that looks like a stack with a star on top of it
* notice how the scene toolbar settings change when you jump in and out of prefab asset mode

Changing a prefab's scale will also change the scale of all its instances in any scene
* however, each instance uses its own position and rotation
* also, game object instances can be modified, which overrides the prefab's values
* note that the relationship between prefab and instance is broken while in play mode
___

## Instantiating Prefabs

Instantiating a game object is done via the `Object.Instantiate` method
* it is a publicly available method of Unity's `Object` type
	* which `MonoBehaviour` is extended from
	* `MonoBehaviour` extends `Behaviour`, which extends `Component`, which extends `Object`
* It clones whatever Unity object is passed to it as an argument

we can do `Transform point = Instantiate(pointPrefab);`
___

# C# 2

Can we multiply structs and numbers?
* normally we can't, but it's possible to define such functionality
	* done by creating a method with a special syntax
	* so the function can be invoked as if it were a multiplication
		* so code is faster and easier to write and read
		* not essential, but nice to have, so it's a syntactic sugar
* `Vector3.right * 2f` is actually a function
	* something like `Vector3.Multiply(Vector3.right, 2f)`
	* the result is a vector equal to all `Vector3.right`'s components doubled

To put multiple attributes on a single field, we can do either
```c#
[SerializeField]
[Range(10, 100)]
int resolution = 10;
// or
[SerializeField, Range(10, 100)]
int resolution = 10;
```

* all `Range` attribute does is intruct the inspector to use a slider with the specified range
* it doesn't affect the field `resolution` in any other way
	* so it doesn't stop us from writting code to assign it a value out of the range

`transform` property inherit from type `Component`

When a new parent is set, Unity will attempt to keep the object at its original world position, rotation, and scale
* we can signal Unity to not do that by passing `false` as a second argument to `SetParent`
* `point.SetParent(transform, false);`
___

# Surface Shader

We want to use a point's position to determine an object's color
* a straightforward way, is to adjust the color property of the material of each cube, with a loop
	* end up with one unique material instance per object
	* later when we animate the graph later, we'd have to adjust these materials all the time
* yes it works, but it isn't very efficient
* it'd be much better if we could use a single material that directly uses the position as its color
	* unfortunately, Unity doesn't have such a material, so let's make our own

> [!info]
> The GPU runs shader programs to render 3D objects. Unity's material assets determine which shader is used and allows its properties to be configured.

To create a custom shader, *Assets / Create / Shader / Standard Surface Shader*
* we can open a shader asset like a script
* which uses different syntax than C#
* by default it contains a surface shader template

Unity provides a framework to quickly generate shaders that perform default lighting calculations, which we can influence by adjusting certain values.
* Such shaders are known as surface shaders
* they only work for the default render pipeline, but not the Universal render pipeline or the likes
___

# Shader syntax

Unity has its own syntax for shader assets
* overall roughly like C#, but it's a mix of different languages

1. begins with `Shader` keyword
* followed by a string defining a menu item for the shader
	* written inside double quotes
* after that comes a code block for the shader's contents

2. Shaders can have multiple sub-shaders, each defined by the `SubShader` keyword followed by a code block

3. below the sub-shader, we also want to add a fallback to the standard diffuse shader
	* by writing `FallBack "Diffuse"`


## Sub-shader

this section needs to be written in a hybrid of CG and HLSL, two shader languages
* enclosed by the `CGPROGRAM` and `ENDCG` keywords

1. The first needed statement is a compiler directive, known as a pragma
* written as `#pragma` followed by a directive
* eg. `#pragma surface ConfigureSurface Standard fullforwardshadows`
	* it instructs the shader compiler to generate a surface shader with standard lighting and full support for shadows
	* `ConfigureSurface` refers to a method used to configure the shader
		* which we'll have to create

* the word pragma comes from Greek
	* refers to an action or something that needs to be done
* it's used in many programming languages to issue special compiler directives

2. we follow that with the `#pragma target 3.0` directive
* it sets a minimum for the shader's target level and quality

3. since we want the material color to change based on the object's world position, it means the surface shader takes an input, which is the world position
* then we have to define the input structure for our configuration function

* written as `struct Input`, followed by a code block and then a semicolon
* inside the block we declare a single struct field, `float3 worldPos`
	* it will contain the world position of what gets rendered, the object
	* `float3` type is the shader equivalent of the `Vector3` struct

* note that this position is determined per vertex
	* for a cube, that is for each corner
* the color will be interpolated across the cube's faces
	* the large the cubes are, the more obvious this color transition will be

### ConfigureSurface

4. Next we define our `ConfigureSurface` method
* actually, in the case of shaders, it's always referred to as a function, not as a method
* it's a `void` function with two parameters
	* first is an input parameter that has the `Input` type that we just defined
	* the second is the surface configuration data, with the type `SurfaceOutputStandard`
		* it must have the `inout` keyword written in front of its type
		* it indicates that it's both passed to the function and used for the result of the function

5. at this point, we have a functioning shader
* so we can now create a material for it
* although the material is currently solid matte black

6. we can make it look more like the default material, by setting `surface.Smoothness` to 0.5, in our configuration function
* note that when writing shader code, we do not have to add the f suffix to `float` values

7. we can also make smoothness configurable
* as if adding a field for it and using that in the function
* the default style is to prefix shader configuration options with an underscore and capitalize the next letter, for example `_Smoothness`

* to make this configuration option appear in the editor, we have to add a `Properties` block at the top of the shader, above the sub-shader block
* then write `_Smoothness` in there, followed by `("Smoothness", Range(0,1)) = 0.5`
	* This gives it the *Smoothness* label, exposes it as a slider with the 0-1 range, and sets its default to 0.5
___

# Coloring

To adjust the color, we have to modify `surface.Albedo`
* in our case, we just directly use the object's position for albedo
	* since both have three components
* `surface.Albedo = input.worldPos;`

> [!info] Albedo
> Albedo means whiteness in Latin. It's a measure of how much light is diffusely reflected by a surface. If albedo isn't fully white, then part of the light energy gets absorbed instead of reflected.

now the world X position controls the point's red color component
the Y position controls the green color component
the Z controls blue

now at world position zero, the albedo is black already
but since the purpose of our example is to plot a graph, and our X domain is -1 to 1, so we want the black is placed at $(-1, -1)$
* simply just do `surface.Albedo = input.worldPos * 0.5 + 0.5;`
* so that the albedo is 0.5 when world position is 0

since our graph just has X and Y domain, we don't really need the blue color
* `surface.Albedo.rg = input.worldPos.xy * 0.5 + 0.5;`
	* this way the blue component stays zero
___

# Demo 2 - shader code

```ShaderLab
Shader "Graph/Point Surface" {
	
	Properties {
		_Smoothness ("Smoothness", Range(0,1)) = 0.5
	}
	
	SubShader {
		CGPROGRAM
		#pragma surface ConfigureSurface Standard fullforwardshadows
		#pragma target 3.0
		
		struct Input {
			float3 worldPos;
		};
		
		float _Smoothness;
		void ConfigureSurface (Input input, inout SurfaceOutputStandard surface) {
			surface.Albedo.rg = input.worldPos.xy * 0.5 + 0.5;
			surface.Smoothness = _Smoothness;
		}
		ENDCG
	}
	FallBack "Diffuse"
}
```
___

# Universal Render Pipeline

Besides the default render pipeline, Unity also has the Universal and High-Definition render pipelines, URP and HDRP for short
* both render pipelines have different features and limitations
* current default render pipeline is still functional, but its feature set is frozen
* In a few years URP will likely become the default

So install the latest *Universal RP* package
* this doesn't automatically make Unity use the URP though

1. first we have to create an asset for it
* via *Assets / Create / Rendering / Universal Render Pipeline / Pipeline Asset (Forward Renderer)*
	* changed as *Rendering / URP Asset (with Universal Renderer)* in 2021
* it will also automatically create another asset for a renderer

2. Next, go to *Project Settings / Graphics*, then assign the URP asset to the *Scriptable Renderer Pipeline Settings* field
* to switch back to the default render pipeline later, simply set *Scriptable Renderer Pipeline Settings* to *None*
* can only be done in the editor, the render pipeline cannot be changed in a built stand-alone app
___

# Shader Graph

our current material only works with the default render pipeline, not URP.
so when URP is used, it's replaced with Unity's error material, a solid magenta.

we have to create a separate shader for the URP
* we could write one ourselves, but that's currently very hard
	* and likely to break when upgrading to a newer URP version
* a better approach is to use Unity's shader graph package to visually design a shader
	* URP depends on this package, so it was automatically installed along with URP package

1. Create a new shader graph
* *Assets / Create / Shader / Universal Render Pipeline / Lit Shader Graph*
	* changed as *Shader Graph / URP / Lit Shader Graph* in 2021 version

2. Open the graph, either by
* double-click the asset in the project window
* press the *Open Shader Editor* button in its inspector

* it's likely cluttered by multiple nodes and panels
* panels are the blackboard, graph inspector, and main preview panels
	* can be resized and can also be hidden via toolbar buttons
* there are also two linked nodes
	* a *Vertex* node and a *Fragment* node
	* they are used to configure the output of the shader graph

3. A shader graph consists of nodes that represent data or operations
* to make a configurable shader property, press the plus button on the *Point URP* backboard panel, then choose *Float*
* that adds a rounded button to the blackboard, representing the property

4. select it and then its configuration can be seen and changed in the *Node Settings* tab in the *Graph Inspector* panel
* *Reference* is the name by which the property is known internally
	* it's essentially the same thing as how we named the property field *_Smoothness* in our surface shader code
	* in fact, we can use the same internal name here as well
* *Default* means default value
* enable *Exposed* toggle, so that materials can get the shader property
* Also we can set the *Mode* to make it appear as input field, slider, toggle, etc.

5. drag the rounded *Smoothness* button from the blackboard onto an open space in the graph
* it will add a smoothness node to the graph
* then connect it to the *Smoothness* input of the *PRB Master node*
	* by dragging from one of their dots to the other, creating a link between them

6. Save the graph, via the *Save Asset* toolbar button

7. Create a material and use the shader
* the shader's menu item is *Shader Graphs / \<asset name>*


## Programming with Nodes
Now we have a working shader, then we can start programming it

8. Create a position node
* open a context menu on an empty part of the graph, then choose *New Node*
* Select *Input / Geometry / Position*
	* or just search for *Position*

* it's set to world space by default
* we can collapse its preview visualization by pressing the upward arrow, that appears when we hover the cursor over it

9. Create a *Multiply* and an *Add* node
* use these to scale the position's XY components by 0.5 and then add 0.5, while setting Z to zero
* these nodes adapt their input types depending on what they're connected to
* connect the nodes and fill in the constant inputs to do the math, then connect the result to the *Base Color* input of *Fragment*

![colored-shader-graph.png (1180×440) (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/basics/building-a-graph/coloring-the-graph/colored-shader-graph.png)

10. we can compact the visual size of the *Multiply* and *Add* nodes
* by pressing the arrow that appear in their top right corner if you hover over them
* it hides all their inputs and outputs that aren't connected to any other node
* this removes a lot of clutter

* also we can delete components of the *Vertex* and *Fragment* nodes
	* via their context menu
* this way we can hide everything that keeps its default value

![compacted-shader-graph.png (1160×388) (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/basics/building-a-graph/coloring-the-graph/compacted-shader-graph.png)

11. sometimes we want to clamp the values
* in this example, since we're setting color so we want to clamp it to 0-1

* if it was surface shader, we would use the `saturate` function
	* it's a special function that clamps all components to 0-1
		* it's a common operation in shaders, known as saturation, hence its name
	* `surface.Albedo.rg = saturate(input.worldPos.xy * 0.5 + 0.5)`

* if it was in the shader graph, we can find the *Saturate* node as well
![saturated-shader-graph.png (920×380) (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/basics/building-a-graph/animating-the-graph/saturated-shader-graph.png)

12. After saving the shader asset, we now get the same colored points that we got when using the default render pipeline

13. extra. if we enter play mode, there is a debug updater appears in a separate *DontDestroyOnLoad* scene in play mode
* it's for debugging URP, and can be ignored

14. If you are curious about the shader code that is generated from the graph, you can get to it via the *View Generated Shader* button of the graph's inspector
___

# Demo 2 - others

Why can't we set `point.localPosition.y`?
* Because it isn't a public field, but a public property
* It passes a copy of the vector value to us, or copies what we assign to it
	* so we're actually adjusting a local vector value

What is `Mathf`?
* It is a struct in the `UnityEngine` namespace
* it contains a collection of mathematical functions and constants
* as it works with floating-point numbers, so its type name was given the *f* suffix

What's a sine wave and $\pi$?
* sine is a trigonometric function, operating on an angle
* let's say we have a circle
	* each point on the circle has an angle $\theta$, associated with it
	* as well as a 2D position

* one way to define the coordinates of those position is $\begin{bmatrix} \sin(\theta) \\ \sin(\theta + \frac{\pi}{2}) \end{bmatrix}$
	* it is the same as $\begin{bmatrix}\sin(\theta) \\ \cos(\theta)\end{bmatrix}$

* and this is the graph
![sin-cos-pix.png (1200×800) (catlikecoding.com)](https://catlikecoding.com/unity/tutorials/basics/building-a-graph/animating-the-graph/sin-cos-pix.png)

* the coordinates, or the graph, represents starting at the top of the circle and going around it in a clockwise direction
![sinecosine.gif (250×253) (sheepolution.com)](https://sheepolution.com/images/book/16/sinecosine.gif)

* the angle $\theta$ is expressed in radians
	* which is the distance the circle's radius can travel along the circumference of the circle
* at the halfway point the traveled distance is equal to $\pi$, or $\pi\times r$, where $r$ means radius
	* the entire circumference has a length of $\pi \times 2r$
* so, $pi$ is actually the ratio between a circle's circumference and it's diameter
* [[How to LÖVE - 3#Angle|here is a more detailed explaination]]
___
