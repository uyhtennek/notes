# Turn off Unusing things
In general, it is best to turn off everything that you don't need in the moment. But we offen do this because it is not convienient to manage all those objects, scripts, audio source, etc.

Therefore, __DO__ turn off some of the expensive components:
* Colliders
	* Even though it is not colliding with anything, the game engine still keep calculating its physics outcome per frame as long as it is not turned off.
	* In extreme case, it can freeze the game. If this is the case, the game didn't crash but it never finish calculating a frame so it won't move at all.
	* To escape from processing physics stuff, try use `Physics.autoSimulation = false` or `Time.timeScale = 0` through 'Immediate Window' in Visual Studio. (should work but never tested)

Additionally, Hertz taught that actually we can destroy the components when we don't need it for a while. Next time we need it just instantiate a new one. It comes with a few benefits:
1. Fewer components is even more performance-friendly than just turning off.
2. The newly instantiate component is always clean.
___
# Object Pooling for UI
Sometimes some UI stuffs are structured like 1 cell per 1 data item. And then there are thousands of UI cells when there are thousands of items. Then, when ever setting them to active or inactive or instantiating them will cost thousands CPU calls.
A solution would be use object pooling for the UI cell so that only a decent of UI cells are loaded instead of all thousands of them.
___
