`cd [location]`
`cd [directory]\[location]`
change directory
`cd ..`
`cd ../..`
go to parent directory
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
___
`dir`
view all files and directories inside the current directory
![[cmd-dir.png]]
* if an item is a file
	* no `<DIR>`
	* has a number (`53` in the example)
		* which is the size of the file
		* in bytes
* there is a _summary_ in the bottom
___
`dir`
to view the current directory
`dir [location]`
to see any directory
* so we don't have to change the current directory to see other directory
`dir /a`
show all things, including hidden items
`dir [any]`
`dir *.png`
show all things with that word in it
___
`wmic logicaldisk get name`
see all the drives in the computer
___
`[drive name]:`
switch to another drive.
notes that `cd ..` can only get us to the root directory of the current drive. but there is none direcotry containing all the drives.
___
`tree`
pretty much like a `dir` command. but instead it lists the structure of the whole drive. notes that it will only list directories but not any contents.
___
