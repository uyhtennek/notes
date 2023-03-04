[hump.timer — hump 1.0 documentation](https://hump.readthedocs.io/en/latest/timer.html#)
* timer
	* schedule the execution of functions
	* eg 1. move critters every 5 seconds
	* eg 2. make the player invincible for a short amount of time
* tweening
___

# Functions

`Timer.new()`
* return a new instance of timer
	* not necessary. There is a global timer, and there are these local timers that we can create.
	* the 2 don't affect each other

> [!notes]
> local timers use `instance:after(1, foo)`
> global timer uses `instance.after(1, foo)`

`Timer.after(delay, func)`
* `delay` in seconds
* returns the timer handle
* there is no guarantee that the delay will not be exceeded
	* just the function will not be executed before the delay has passed

`Timer.script(func(wait))`
```lua
Timer.scrit(function(wait)
	print("Now")
	wait(1)
	print("After one second")
	wait(1)
	print("Bye!")
end)
```

`Timer.every(delay, func, count)`
`Timer.during(delay, func, after)`

`Timer.cancel(handle)`
* cancel a timer

`Time.clear()`
* cancel all timers
___

# Tweening
short for *in-betweening*

`Timer.tween(duration, subject, target, method, after, ...)`
* returns a timer handle

```lua
function love.load()
	color = {0, 0, 0}
	Timer.tween(10, color, {255, 255, 255}, 'in-out-quad')
end

function love.update(dt)
	Timer.update(dt)
end
```
___
