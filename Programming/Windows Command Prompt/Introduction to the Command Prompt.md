[Windows Command Line Tutorials - YouTube](https://www.youtube.com/playlist?list=PL6gx4Cwl9DGDV6SnbINlVUd0o2xT4JbMu)
___
# How to open?
Windows > type 'cmd' > Enter

# Why do we need it?
* keyboard is faster than mouse
* some tools and softwares are only available in command prompt
	* like security and networking tools
* to build tool
___
`tab` to auto-complete
it looks at alphabetical order
```Example
My Directory
 > Adobe
 > AutoCAD
 > Alphabet
```
type `A` and hit `tab`
	1st  `tab`. Adobe
	2nd `tab`. AutoCAD
	3rd  `tab`. Alphabet
___
command prompt can't have space
`cd C:\Program Files` --> `cd "C:\Program Files"`
___

# PATH variable
To open or execute a file, we need to get into the directory that containing the file, and type in the name of that file. But actually we can run that file or program from anywhere, using **Windows path**.

So what happen when we enter a file name in command prompt?
It will try to find a matching result with the name in the current directory. If it doesn't find it, it does nothing. But if we have something in the PATH variable, the command prompt will also look through all the path in PATH variable and see if any matching result can be found. If there is, the program can still run.

Usually what is in the PATH variable is a bunch of directories.
Also aside from the command prompt, we can also look them up in Windows GUI.
![[cmd-path-gui.png]]
___

# File Attributes
type `attrib /?` in command prompt to see all the available attributes

> A for Archive
> whether or not a file needs to be backed up

> H for Hidden
> whether or not to hide a file.
> hidden files will not show up when we run `dir`.
> usually just to prevent people from deleting them.

> R for Read-only
> it means this file can only be opened and viewed but not written or deleted.

> S for System
> it means this file is needed by Windows.