# MMFeedback
The core component of FEEL

1. Use an empty GameObject to attach a MMFeedback component

2. Add new feedback... > ....
* every feedback has a bunch of settings

3. Assign the target component

4. Many feedbacks have some curves
* curves have a value of 0 - 1
* we set the `Remap Curve Zero` and `Remap Curve One` to map the 0 - 1 to different numbers

5. We can test the feedbacks by first going into Play mode and press Play on the MMFeedbacks component

6. If we want the feedback to play on the game object did something, maybe via code
* we would also need to play the feedback in code
	* just assign the MMFeedbacks reference
	* and then do `<mmfeedbacks>?.PlayFeedbacks()` when ever needed
___

# More feedbacks

* we can order feedbacks
* we can rename feedbacks
	* by changing the Label property


## Camera Shake

Camera > Cinemachine Impulse
* Raw Signal > 6D Shake
* Play Method > Cached
* Velocity > 1, 1, 1

In our Cinemachine Virtucal Camera,
* Add Extension > CinemachineImpulseListener
___

# Post Processing

1. Create a Game Object, named PostProcessingVolume

2. Add a Post-process Volume component
* set it to Is Global
* create a new Profile by pressing New, next to the field

3. On our main camera, add a Post-process Layer
* Layer > Everything
	* just ignore the warning
* Anti-aliasing Mode > Fast Approximate Anti-aliasing (FXAA)

4. Back to our Post-process Volume
* Add effect... > Unity > Vignette
	* Intensity > 0.35
* Add effect... > Unity > Ambient Occulusion
	* click All
	* Intensity > 0.1
	* change color
* Add effect... > Unity > Lens Distortion
	* click All
* Add effect... > Unity > Chromatic Aberration
	* click All

There are MM scripts that specifically touch on Post-process Volume, they are the Shakers
* eg. MMLensDistortionShaker
* they listen for event and shake something in post-process volume

5. add that shaker to the PostProcessingVolume game object
6. same goes to MMChromaticAberrationShaker

7. Go to our LandingFeedback
* Add new feedback... > PostProcess > Chromatic Aberration
* Add new feedback... > PostProcess > Lens Distortion
___

# MMTools
many are not correctly documented yet though

## MMAutoRotate
Create a game object, and attach to it a MMAutoRotate component
* Rotation Speed > 0, 50, 0
* Check Orbiting
	* assign a Transform to Orbit Center Transform
	* note that it will immediately show a orbit gizmo
* It should look better if we set Update Mode to Fixed Update in this case

## MMFollowTarget
Create a game object, and attach to it a MMFollowTarget component
* Assign a Transform to it as Target
* Uncheck Follow Position X and Follow Position Z
* Uncheck Follow Scale
* Check Add Initial Distance Y to Y Offset
* maybe set Follow Position Mode to MM Spring to make it look a bit more interesting
	* instead of MM Lerp

## MMFPS Counter
Create a UI > Text, attach the component

## MMFPS Unlock
it unlocks the Unity's default FPS limit
___

# Using Unity Events

So maybe we want to use `<event>?.Invoke()` instead of `<feedbacks>?.PlayFeedbacks()`
* maybe we are using a framework that accepts Unity Events, or
* maybe we are using BOLT or any visual scripting tool

Just drag the MMFeedbacks to the Unity Event
* then select MMFeedbacks > PlayFeedbacks
___
