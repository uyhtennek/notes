`[file name]`
open file with the *default* program
`.exe` file will run on command prompt instead of other software
`Ctrl + C` to stop running the executable
___
`[command] /?`
brings up a help or option menu about the `[command]`
___
`mkdir [name]`
make a directory with a name `[name]`
`rmdir [name]`
remove a directory with the name `[name]`
however this can only delete an empty directory
`rmdir /s [name]`
instead add `/s` to delete a directory with stuffs inside it
___
`path`
show all added path variables
> when ever we typed in a command, terminal will first look through the current directory. if in the end none matching result is found, then also look through the path variable.

```shell
C:\Users\Administrator>path

# the following result is tidied for readability as the true result is messy
PATH=
C:\Windows;
C:\Windows\system32;
C:\Windows\System32\Wbem;
C:\Windows\System32\WindowsPowerShell\v1.0\;
C:\Windows\System32\OpenSSH\;
C:\Program Files\Git\cmd;
C:\Program Files\Microsoft VS Code\bin;
C:\Program Files\nodejs\;
C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;
C:\Program Files\NVIDIA Corporation\NVIDIA NvDLISR;
C:\Program Files\dotnet\; C:\Users\Administrator\AppData\Local\Microsoft\WindowsApps;
C:\Users\Administrator\AppData\Roaming\npm
```
___
`color [hex digit 1][hex digit 2]`
change color of the command prompt, including font color and background color
`color`
get back to default colors
`color /?`
we can see what `[hex digit]` refer to what color using `/?` in this case.
`[hex digit 1]` set the background color
`[hex digit 2]` set the foreground, or text, color
___
`cls`
clear screen
`*`
it means any, like any word
___
**up/down** arrows
going forward or backword in the command history = copy last / next command
___
