[FFmpeg Encoding and Editing Course (slhck.info)](http://slhck.info/ffmpeg-encoding-course/#/)
___

# FFmpeg

free, open-source software for multimedia editing, conversion, ....
started in 2000

similar or related frameworks
* ImageMagick
* MLT Framework

it contains
* command-line tools: `ffmpeg`, `ffprobe`, `ffplay`
* libraries: `libavformat`, `libavcodec`, `libavfilter`, ....
	* used in many projects, eg. VLC, MLT Framework, ....
	* can be used from C/C++ code
___

# Libraries

`libavformat`
* reads and writes container formats
* eg. AVI, MKV, MP4, ....

`libavcodec`
* reads and writes codecs
* eg. H.264, H.265, VP9, ....

`libavfilter`
* various filters for video and audio

.... and many more

examples on how to use libraries
[Lei Xiaohua's learning resource about video/audio technics (leixiaohua1020.github.io)](http://leixiaohua1020.github.io/#ffmpeg-development-examples)

similar or related libraries
* OpenCV
* Python - MoviePy, `pyav`, `scikit-video`
___

# Installation

Linux Packages
```bash
sudo apt-get install ffmpeg
```
___

# Encoding Concepts

1. Containers **contain** media data.
* eg. MP4: MPEG-4 Part 14 container for H.264, H.264, AAC audio, ....
* eg. MKV: Versatile container for any media format
* eg. WebM: Subset of MKV, usage in Web streaming
* eg. AVI: Legacy container

see supported containers with
```bash
ffmpeg -formats
```

2. CODEC = Coder / Decoder
* specify how to code and decode video, audio, ....
* usually not specifying how to encode / compress data
* sometimes "codec" means an actual encoding / decoding software

see supported codecs with
```bash
ffmpeg -codecs
```

currently mostly used **Lossy** CODECs, standardized by ITU/ISO
* H.262 / MPEG-2 Part H
	* Broadcasting, TV, used for backwards compatibility
* H.264 / MPEG-4 Part 10
	* The de-facto standard for video encoding today
* H.265 / HEVC / MPEG-H
	* Successor of H.264, up to 50% better quality

* MP3 / MPEG-2 Audio Layer III
	* used to be the de-facto audio coding standard
* AAC / ISO/IEC 14496-3:2009
	* advanced audio coding standard

competitors that are royalty-free
* VP8
	* free, open-source codec from Google
	* not so much in use anymore
* VP9
	* successor to VP8
	* almost as good as H.265
* AV1
	* successor to VP9
	* claims to be better than H.265

3. Encoders are the actual software that outputs a codec-compliant bitstream
* libx264
	* most popular free and open-source H.264 encoder
* NVENC
	* NVIDIA GPU-based H.264 encoder
* libx265
	* free and open-source HEVC encoder
* libvpx
	* VP8 and VP9 encoder from Google
* libaom
	* AV1 encoder
* libfdk-aac
	* AAC encoder
* aac
	* native FFmpeg AAC encoder
* ....

see supported encoders
```bash
ffmpeg -encoders
```

4. pixel formats
* [Chroma subsampling - Wikipedia](https://en.wikipedia.org/wiki/Chroma_subsampling)
* 4:4:4
	* full horizontal resolution, full vertical resolution

see supported pixel formats
```bash
ffmpeg -pix_fmts
```
___

# `ffmpeg`

General syntax
```bash
ffmpeg <global-options> <input-options> -i <input> <output-options> <output>
```

`<global-options>`
* for log output, file overwriting, ....

`<input-options>`
* for reading files

`<output-options>`
* for conversion configurations
	* eg. codec, quality, ....
* for filtering
* for stream mapping
* .....

Full help
```bash
ffmpeg -h full
# or, the huge one
man ffmpeg
```


## examples

transcoding from one codec to another
* `libx264` = H.264
```bash
ffmpeg -i <input> -c:v libx264 output.mp4
```

transmuxing from one container/format to another
* without re-encoding
* `ffmpeg` will take one video, audio, and subtitle stream from the input and map it to the output
```bash
ffmpeg -i input.mp4 -c copy output.mkv
```

`-c`
* sets the encoder
* `-c:v` sets only video encoder
* `-c:a` sets only audio encoder
* `-c copy` only copies bitstream

`-an` and `-vn`
* disable audio or video streams

> [!info] `ffmpeg` workflow
> 1. read input files
> 	* get packets containing encoded data from the files
> 	* when there are multiple input files, `ffmpeg` tries to keep them synchronized
> 2. pass the encoded packets to the decoder
> 3. the decoder produces uncompressed frames
> 4. apply filtering to the frames
> 5. pass the frames to the encoder
> 	* it encodes the frames and outputs encoded packets
> 6. pass the packets to the muxer
> 	* writes the encoded packets to the output file

cutting a video
```bash
ffmpeg -ss <start> -i <input> -t <duration> -c copy <output>  # <duration> from <start>
ffmpeg -ss <start> -i <input> -to <end> -c copy <output>      # from <start> to <end>

# eg.
ffmpeg -ss 00:01:50 -i <input> -t 10.5 -c copy <output>
ffmpeg -ss 2.5 -i <input> -to 10 -c copy <output>
```
___

## Setting Quality

* the output quality depends on encoder defaults and the source material
* do not just encode without setting any quality level
* generally, we need to choose a target bitrate or quality level

set bitrate
* `-b:v` or `-b:a`
* eg. `-b:v 1000K` = 1000 kbit/s
* eg. `-b:v 8M` = 8 Mbit/s

set fixed-quality parameter
* `-q:v` or `-q:a`
* eg. `-q:a 2` for native AAC encoder

encoder-specific options
* `libx264` / `libx265`
	* `-crf`
		* = Constant Rate Factor
		* maintain constant quality across the entire encode
		* good for storing video at fixed quality if file size is not important
* FDK-AAC encoder
	* `-vbr`
		* = constant quality
* .... and more
	* eg. can see from `ffmpeg -h encoder=libx264`

.....
