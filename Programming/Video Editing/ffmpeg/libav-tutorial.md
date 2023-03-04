[FFmpeg libav tutorial - learn how media works from basic to transmuxing, transcoding and more.](https://github.com/leandromoreira/ffmpeg-libav-tutorial)
___

# Intro

video = a series of pictures / frames running at a given rate

audio
![68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f632f63372f4350542d536f756e642d4144432d4441432e7376672f36343070782d4350542d536f756e642d4144432d4441432e7376672e706e67 (640×220) (camo.githubusercontent.com)](https://camo.githubusercontent.com/645b5042af302a7bdd2ed384d9efe582b390117ddad7acdf97cf584893da88b0/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f632f63372f4350542d536f756e642d4144432d4441432e7376672f36343070782d4350542d536f756e642d4144432d4441432e7376672e706e67)

but if we chose to pack millions of images in a single file and called it a movie, we might end up with a hugh file
```c
toppf = 1080 * 1920  // resolution
cpp = 3              // 3 bytes per pixel = 24 bit color
tis = 30 * 60        // 30 minutes long
fps = 24             // 24 frames per second

required_storage = tis * fps * toppf * cpp
// it needs: storage - 250.28 GB; bandwidth - 1.19 Gbps
```

that's why we need to use a CODEC
* it converts raw (uncompressed) digital audio/video to a compressed format
* or vice versa

container
* a single file that contains all the streams (= audio/video)
* also provides metadata like title, resolution, and etc.
* eg. a `video.webm` is probably a video using the container `webm`
![container.png (621×328) (raw.githubusercontent.com)](https://raw.githubusercontent.com/leandromoreira/ffmpeg-libav-tutorial/master/img/container.png)
___

# FFmpeg - command line

FFmpeg is a multimedia tool/library
* it has a command line program called `ffmpeg`

to convert from `mp4` to the container `avi`
```bash
ffmpeg -i input.mp4 output.avi
```

* this is known as *remuxing*
	* converting from one container to another one


## command line tool 101
[ffmpeg Documentation](https://www.ffmpeg.org/ffmpeg.html)
[[encoding-course]]

> [!tip] browse documentation
> to open up documentation from command line
> ```bash
> ffmpeg -h full | grep -A 10 -B 10 avoid_negative_ts
> ```

`ffmpeg` expects the following arguments
```bash
ffmpeg {global options} {input options} -i {input url} {output options} {output url}
```

eg.
```bash
ffmpeg \
-y \                             # global options
-c:a libfdk_aac \                # input options
-i bunny_1080p_60fps.mp4 \       # input url
-c:v libvpx-vp9 -c:a libvorbis   # output options
bunny_1080p_60fps_vp9.webm       # output url
```
* input file is a `mp4`, containing two streams
	* an audio encoded with `aac` CODEC; a video encoded using `h264` CODEC
* convert it to `webm`
	* changing its audio and video CODECs as well

we can simplify the command above
* be aware that `ffmpeg` will adopt or guess the default values for us
* eg. `ffmpeg -i input.avi output.mp4`
	* it will use the default audio/video CODEC to produce `output.mp4`
___

## Common operations

Transcoding
![transcoding.png (494×225) (raw.githubusercontent.com)](https://raw.githubusercontent.com/leandromoreira/ffmpeg-libav-tutorial/master/img/transcoding.png)
* converting one of the streams from one CODEC to another one

```bash
ffmpeg \
-i bunny_1080p_60fps.mp4 \
-c:v libx265 \
bunny_1080p_60fps_h265.mp4
```


Transmuxing
![transmuxing.png (492×231) (raw.githubusercontent.com)](https://raw.githubusercontent.com/leandromoreira/ffmpeg-libav-tutorial/master/img/transmuxing.png)
* converting from one format / container to antoher one

```bash
ffmpeg \
-i bunny_1080p_60fps.mp4 \
-c copy \ # skip encoding
bunny_1080p_60fps.ts
```


Transrating
![transrating.png (437×235) (raw.githubusercontent.com)](https://raw.githubusercontent.com/leandromoreira/ffmpeg-libav-tutorial/master/img/transrating.png)
* changing the bit rate, or producing other renditions
* 