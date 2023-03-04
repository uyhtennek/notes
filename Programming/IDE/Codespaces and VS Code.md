[Developing Your Project Locally with VS Code (Windows) - CS50 Seminars 2021](https://video.cs50.io/9yzQCgIdL-Y)
___

# Codespaces

actually we've been using VS Code through https://code.cs50.io
* but it's not connecting to our own computer
* it's connecting to a *codespaces*

Codespaces is essentially a place we can harvest or use computing resources through the cloud
* eg. GitHub and Microsoft have hosted computers that we can connect to and use
* CS50 is using GitHub Codespaces
	* if we don't like connecting to it via browser
	* we can connect to it via VS Code
		* there is a checkbox saying <input type="checkbox">Open in VS Code Desktop

In CS50 case, the remote computers are
1. running Linux (Ubuntu 20.04)
2. pre-installed with some software packages
	* Python 3, NodeJS, http-server, Flask, sqlite3, ...
___

# Why local?
Codespaces seems neat. Why use local development environment?

it's much more flexible
* we can choose the version of the software packages, and we can choose the software packages
* we don't rely on Internet connection anymore

CS50's codespaces also disabled git commands
* if we try to do that it'd get mad
* `You are in a repository managed by CS50. Git is disabled.`
___

# VS Code and WSL

Terminal in VS Code for Windows is a bit different
it's using the Windows PowerShell
* it's not using Linux, so Linux commands don't necessarily work on our Windows PCs

If we want to use Linux commands then we need Linux system
so [download Linux](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-11-with-gui-support#1-overview), which formally known as Ubuntu

and we also connect VS Code to our Linux subsystem
1. by clicking on the button "Open a Remote Window"
2. then in the prompt select "Open Folder in WSL..."

also since Git is installed in Ubuntu, we can use Git commands now
___

# Update software packages

`apt-get`
* `apt` stands for *Advanced Packaging Tool*
* it's a program to get code or things in the wild, out on the Internet, onto our own local computer
* allows us to get things like Python, sqlite3, etc.

`sudo`
* says to the computer that "I'm an administrator. I know what I'm doing."
* it's a prefix to a command
	* it makes sure we run the command with administrator privileges
* similar to if it was Windows it'd have a pop-up asking "Are you sure you want to run this program? Do you have administrative privilege?"
	* then we hit Continue
	* `sudo` is the same thing, but it is in the terminal

`sudo apt-get update`
* updates `apt-get` with administrator privileges


Here are some softwares or libraries used for CS50x course
which you may find helpful

1. Python 3
* `sudo apt-get install python3`

2. pip
* stands for *Python Package Index*
* a Python package manager, from PyPI
* we use it to install all of these Python libraries
	* we can go to [PyPI](https://pypi.org/) website to search for libraries
* `sudo apt-get install python3-pip`

3. flask and flask_session
* `pip3 install flask`
	* also flask already included some other libraries like `tempfile`, `werkzeug`
	* when pip installs a package, it'll recursively look for if there is any other dependency for that package and install those extra packages as well
* `pip3 install flask_session`

4. CS50
* `pip3 install cs50`

5. sqlite3
* `sudo apt-get install sqlite3`

It's `pip3` but not `pip`
* `pip3` is the latest version, `pip` and `pip2` are old
___
