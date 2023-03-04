# How to introduce new feature _SAFELY_
> It's easy to make mistake when developing new feature. More importantly, sometimes mistakes are made in a way that prevent the __current working functions__ from functioning.
> So, it's always a good practice to be careful when implementing new features. __Better to be SAFE than Sorry.__
---
### git branch

Only merge branch WHEN it's time to put it into use.

Just don't interupt old code unless necessary. Although we can choose to hide the new code before publishing, then why not make those code don't exist in the working branch in the first place?
___
### safe checks

Checks are essential to introduce in all scenarios besides introudcing new features. Wrong data can easily mess up the code after all, like NULL or index that larger than the array size.

Reminder of common things that may need checks:
* Singleton
`GameManager.instance`
>just commonly be missing due to many reason
* Array / Array List / Dictionary / etc.
`arr[i]`

Hertz said usually just make a check when ever needed, instead of have a place to run the check before using them. I guess the latter is redudant maybe?
___
### Define Symbol
[Unity - Manual: Custom scripting symbols (unity3d.com)](https://docs.unity3d.com/Manual/CustomScriptingSymbols.html)
