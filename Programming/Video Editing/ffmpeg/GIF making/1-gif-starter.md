[cyburgee/ffmpeg-guide (github.com)](https://github.com/cyburgee/ffmpeg-guide)
___

# Video To GIF

1. simplest
```bash
ffmpeg -ss <start> -t <duration> -i <input> -f gif output.gif
```
* big file size
* quality probably is not good


2. generate and use better color palette
```bash
ffmpeg -ss 61.0 -t 2.5 -i StickAround.mp4 \
	-filter_complex "[0:v] palettegen" palette.png
ffmpeg -ss 61.0 -t 2.5 -i StickAround.mp4 -i palette.png \
	-filter_complex "[0:v][1:v] paletteuse" prettyStickAround.gif
```

`-filter_complex "[0:v] palettegen"`
* `palettegen` is a filter that generates a 256 color palette
* it's used in the next step, which is the GIF encoding
* `[0:v]` means it uses the first video stream of the input
* we named the output file `palette.png`

`-filter_complex "[0:v][1:v] paletteuse"`
* `paletteuse` filter takes in two arguments
	* the first is the video content
	* the second is the color palette


3. skip managing the intermediate palette file
```bash
ffmpeg -ss 61.0 -t 2.5 -i StickAround.mp4 \
	-filter_complex "[0:v] split [a][b];[a] palettegen [p];[b][p] paletteuse" \
	FancyStickAround.gif
```

`[0:v] split [a][b]`
* the `split` filter
* takes the first video as input, creates two outputs from the one input
* `[a]` and `[b]` can be named anything, eg. `[dog][cat]`

`;[a] palettegen [p]`
* the semicolon `;` indicates that we're specifying a new filter
* it's the `palettegen` filter
	* takes `[a]` as input and output `[p]`
* `[p]` again can be named anything

`;[b][p] paletteuse`
* `[b][p]` are the inputs
* order is important

the drawback here is that we're using more memory at once
* not a big deal though


4. diminish file size
```bash
ffmpeg -ss 61.0 -t 2.5 -i StickAround.mp4 \
	-filter_complex "[0:v] fps=12,scale=480:-1,split [a][b];[a] palettegen [p];[b][p] paletteuse" \
	SmallerStickAround.gif
```

`[0:v] fps=12,scale=480:-1,split [a][b];`
* filters that accept a single input and output are allowed to be separated by commas `,`
* the `scale` filter needs two parameters, width and height
	* it can be written in several forms
		* `scale=w=480:h=-1`
		* `scale=width=480:height=-1`
		* `scale=480:-1`
	* we want to scale the input video to width 480 pixels and auto-determine the height

* `-1` for the `scale` filter means, maintain the aspect ratio while resizing the other dimension we specified
* `-2` has a similar meaning - to stay as close to the aspect ratio but round to an even number
	* because many video codecs require even dimensions, eg. H.264
	* for GIFs though, it doesn't matter

* if you're change the frame rate and resizing the video, resize before increasing the framerate, or resize after decreasing the framerate
	* so that our computer won't have to resize as many frames
	* saving us time and processing power


5. high quality GIF
* 256 colors per frame, so one color palette for each frame
```bash
ffmpeg -ss 61.0 -t 2.5 -i StickAround.mp4 \
	-filter_complex "[0:v] fps=12,scale=w=480:h=-1,split [a][b];[a] palettegen=stats_mode=single [p];[b][p] paletteuse=new=1" \
	StickAroundPerFrame.gif
```

`[a] palettegen=stats_mode=single [p]`
* `stats_mode=single` tells the filter to generate a new palette for every input frame

`[b][p] paletteuse=new=1`
* `new=1` tells the filter to grab a new palette for each frame

the drawback is that it increases the file size
* weird enough, it can also decrease the file size
___
