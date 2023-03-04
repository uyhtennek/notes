# Make scripts run in an exact order

### Is it neccessary?
Hertz said we seldom need to do this. Even we do, setting `scripts execution order` should be okay.
I guess maybe only bad code need to do this...

### Sound-Good Practice
Hertz said we can make a dedicated manager to handle functions like Start(), Update(), OnApplicationPause() if we're worried. Bato did this before to handle Update() of some objects in a scene, because he's cloning Celeste, a hardcore platformer.