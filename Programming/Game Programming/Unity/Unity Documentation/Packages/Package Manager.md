[Unity's Package Manager](https://docs.unity3d.com/2022.2/Documentation/Manual/Packages.html)
___

# How packages work

1. When Unity opens a Project, the Unity Package Manager reads the Project manifest
* to figure out what packages to load in the Project

* each project has its own manifest which lists the packages to load for the Project
	* as if the packages are "dependencies" of the Project

2. It sends a requests to the package registry server, for each package that appears as a dependency in the manifest

3. The package registry then sends the requested information and data to the Package Manager

4. It then installs those packages

![upm-overview.png (473×424) (unity3d.com)](https://docs.unity3d.com/2022.2/Documentation/uploads/Main/upm-overview.png)
___
