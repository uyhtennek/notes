[How do I convert a video to GIF using ffmpeg, with reasonable quality? - Super User](https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality)
___

# Short Answer

```bash
ffmpeg -ss 30 -t 3 -i <input> \
	-vf "fps=10, scale=320:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" \
	-loop 0 output.gif
```

`-ss 30`
* skip the first 30 seconds

`-t 3`
* create a 3 second output

`-vf <filter>`
* `fps=10`
	* 10 frames per second
* `scale=320`
	* resize the output to 320 pixels wide
	* automatically determine the height while preserving the aspect ratio
	* `flags=lanczos` is the scaling algorithm
* `palettegen` and `paletteuse`
	* generate a custom palette from the `<input>`, and use that
* `split[s0][s1]`
	* allow everything to be done in one command
	* and avoid having to create a temporary PNG file of the palette

`-loop`
* `0` is infinite looping
* `-1` is no looping
* `1` means loop once
	* so it will play twice
	* so `10` means loop 10 times or play 11 times
___
