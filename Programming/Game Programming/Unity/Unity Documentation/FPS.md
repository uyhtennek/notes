[Application.targetFrameRate](https://docs.unity3d.com/ScriptReference/Application-targetFrameRate.html)
___

# FPS

`Application.targetFrameRate`
* default is `-1`
	* Unity uses the platform's default target frame rate

set it to a value that allows
* the game to run smoothly and consistently
* and to conserve battery life on mobile devices and avoid overheating

note that the game's target frame rate is limited by the platform and device capabilities
___

## V-Sync

`Application.targetFrameRate` controls the game's frame rate
`QualitySettings.vSyncCount` specifies the number of screen refreshes to allow between frames

both control the game's frame rate
___

## Platform notes

`Application.targetFrameRate` causes frame pacing issues on desktop, so on desktop we need to use `QualitySettings.vSyncCount`

mobile platforms ignore `QualitySettings.vSyncCount`, so use solely `Application.targetFrameRate`

VR platforms ignore both `QualitySettings.vSyncCount` and `Application.targetFrameRate`
* it uses the VR SDK to control the frame rate

on all other platforms, Unity ignores `Application.targetFrameRate` if we set `QualitySettings.vSyncCount`
* when we set `vSyncCount`, Unity calculates the target frame rate by dividing the platform's default target frame rate by the value of `vSyncCount`
* eg. if the platform's default render rate is 60 fps, and we set `vSyncCount` to 2, Unity tries to render the game at 30 fps
___

### platform defaults
to use the platform's default frame rate, set `Application.targetFrameRate` to `-1`


**Standalone platforms**
* default frame rate is the maximum achievable frame rate


**Mobile platforms**
* a mobile device's maximum achievable frame rate is the refresh rate of the screen
	* so to target the maximum achievable frame rate, set `Appliction.targetFrameRate` to the screen's refresh rate
	* the information is contained in `Screen.currentResolution`
* default frame rate on mobile usually is 30 fps

* set `Application.targetFrameRate` to the screen's refresh rate divided by an integer
	* it the target frame rate is not a divisor of the screen refresh rate, the result is always lower than `Application.targetFrameRate`
* it also implies the v-sync count
	* note that mobile platforms ignore the `QualitySettings.vSyncCount`
	* if we want a vertical sync count of 1, we set `Application.targetFrameRate` to screen's refresh rate divided by 1
		* eg. if screen's refresh rate is 60 Hz, and we want 2 vertical syncs between frames, set `targetFrameRate` to 30


**WebGL**
* default is the browser's chosen frame rate
* usually the browser choose a number that gives the smoothest performance


**VR**
* ignore both `Application.targetFrameRate` and `QualitySettings.vSyncCount`
* the VR SDK controls the frame rate


**Unity Editor**
* `Application.targetFrameRate` affects only the Game View
* it has no effect on other Editor windows
___

```cs
using UnityEngine;

public class Example
{
	void Start()
	{
		// Use the default fps
		Aplication.targetFrameRate = -1;
		// Limit the framerate to 60
		Application.targetFrameRate = 60;
	}
}
```
___
