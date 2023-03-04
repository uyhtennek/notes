# Fun fact: "de-bug"

![[debug.png]]

In 1947, a computer scientist *Grace Hopper* who happen to be the one to record the very first "bug" in a computer. And it is an actual bug, a moth.

This moth flown into *Harvard Mark II* computer (it's a very large, refrigerator-sized type system), and caused an issue.

This is how the word "bug" comes from. Etymology (詞源) of bug.
___

# Debug tools

1. `printf()`
* can look inside the values of variables, then print them out
* see if the values are expected

2. `debug50`
* it's a command, representing a **debugger** program
* the debugger it used, is actually built in to VS Code. `debug50` is just automating the process of starting VS Code's built-in debugger.
* the debugger can walk through the code step-by-step
	* we can pause the program in any where or any line, and then we can walk through the code one step at a time
	* we pause the execution by adding **break-point**, usually it's a red dot.
	* see [Step through](https://video.cs50.io/---HbbANxDQ)
* VS Code usually open *Debug Console* when running debugger. but we can get back to the normal *Terminal* if we don't need it.
* after adding break-point (otherwise `debug50` consider it has no need to run), the program then will be executed. later the program will pause at the line of where the first break-point is.
	* a *yellow* line in the same line will show, which means this line is yet to be executed.
	* there are some buttons / actions enabled when the program is paused:
		1. Play
		2. Step Over
		   execute the current line (*yellow line*) and then move to the next line
		3. [Step Into](https://video.cs50.io/tk3cl8hyfqM?start=1)
		   step into functions if any
		4. Step Out Of
		   similarly, step out of functions
		* can think as *Step Over ~~function~~, Step Into ~~function~~, Step Out Of ~~function~~*
	* all variables, as well as their values, will be printed, usually in a seperate panel
		* About at the top left area in VS Code
* Any way, debugger is just so much more powerful than `printf()`

3. **rubby duck**
* talking through problems, talking through code with someone else
* talking with step-by-step details, eg.
	1. on line 3, I'm starting a for loop
	2. and I'm initializing i to 0
	3. then I'm printing out a hash
	4. .......
	5. wait a minute, I just said something stupid!
* we don't always have a person who want to hear about our code. so we just get ourselves a proxy, a rubber duck!
* it helps us hear illogic in what we think is logical
___

