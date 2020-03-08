+++
title = "Command-Line Cheat Sheet"
slug = "cmd-cheet-sheet"
date = 2018-07-21T03:58:05+01:00
tags = ["media", "ffmpeg", "video", "batch", "audio", "cmd", "bash", "imagemagick"]
categories = ["Media"]
draft = false
description = "Batch, Bash, FFmpeg and ImageMagick."
+++
How to use various command-line utilities including batch, bash, FFmpeg and ImageMagick.
<!--more-->

## Conversion

### Batch To Mp3

Loop through every video in this directory and convert it to V2 .mp3.
```
for %%a in ("*.mkv" "*.mp4" "*.webm") do ffmpeg -i "%%a" -codec:a libmp3lame -qscale:a 2 "%%a.mp3"
```

### Batch to Opus

Renames to .ogg for compatibility reasons.
Defaults to 96K bitrate.
```
for %%a in ("*.mkv" "*.mp4" "*.webm") do ffmpeg -i "%%a" -acodec libopus "%%a.opus"
ren *.opus *.ogg
```

### Encode VP8 Webm

```
ffmpeg -c:v libvpx -pix_fmt yuv420p -lag-in-frames 16 -auto-alt-ref 1 -qmin 28 -crf 30 -qmax 32 -qcomp 1 -b:v 0 out.webm
```

### Encode to Twitter MP4

```
ffmpeg -i INPUT_FILENAME -c:v libx264 -strict -2 OUTPUT_FILENAME.mp4
```

### Convert to JPEG

Convert all BMP files in directory to JPEG then delete them.
```
magick mogrify -format jpg -quality 80 *.bmp
del *.bmp
```

### Output Frames of Video

```
ffmpeg -i inputvideo.mp4 frames/frame%05d.bmp  
ffmpeg -ss 00:19:20 -i inputvideo.mkv -t 5 frames/frame%05d.bmp
```

## File Editing

### Renaming

`ren *.jpg *@TehLemon.jpg`

## Image Editing

`convert` creates a new file  
`mogrify` overwrites the original file

### Resize

```
magick convert "thing.jpg" -resize 50% "thing-small.jpg"
magick mogrify -resize 50% "thing.jpg" 
```

### Rotation

```
magick convert -rotate "90" in.jpg out.jpg
magick mogrify -rotate "-45" foo.jpg
```

### Tiling

<https://www.imagemagick.org/Usage/montage/#settings>  
Create a spritesheet from the files with a maximum size of 256x128, with a spacing of 0 pixels in between and 1 pixel on the top and bottom, with 2 columns and 1 row.  
Tiling optional, montage will distribute it evenly by default.  
256x128 optional, montage will leave things at their original size.  

```
magick montage "file1.jpg" "file2.jpg" "file3.jpg" -geometry 256x128+0+1 -tile 2x1 montage.jpg
```
```
magick montage *.jpg -geometry +0 -tile 1 foo.jpg
```

## Video Editing

### Hold the last frame

`N` in `trim=0:N` changes the duration of the freeze.
```
ffmpeg -i in.webm -filter_complex "[0]trim=0:N[hold];[0][hold]concat[extended];[extended][0]overlay" -c:v libvpx -pix_fmt yuv420p -lag-in-frames 16 -auto-alt-ref 1 -qmin 28 -crf 30 -qmax 32 -qcomp 1 -b:v 0 out.webm
```

### Tiling

```
ffmpeg
	-i 1.avi -i 2.avi -i 3.avi -i 4.avi
	-filter_complex "
		nullsrc=size=640x480 [base];
		[0:v] setpts=PTS-STARTPTS, scale=320x240 [upperleft];
		[1:v] setpts=PTS-STARTPTS, scale=320x240 [upperright];
		[2:v] setpts=PTS-STARTPTS, scale=320x240 [lowerleft];
		[3:v] setpts=PTS-STARTPTS, scale=320x240 [lowerright];
		[base][upperleft] overlay=shortest=1 [tmp1];
		[tmp1][upperright] overlay=shortest=1:x=320 [tmp2];
		[tmp2][lowerleft] overlay=shortest=1:y=240 [tmp3];
		[tmp3][lowerright] overlay=shortest=1:x=320:y=240
	"
	-c:v libx264 output.mkv
```

### Time Range

```
ffmpeg -i input.mp4 -ss 00:00:00.00 -to 00:04:15.00 [output]
```

### Concatenation

<https://trac.ffmpeg.org/wiki/Concatenate>  

Only works for videos of the same codec.  
`list.txt`
```
file '/path/to/file1.webm'  
file '/path/to/file2.webm'  
file '/path/to/file3.webm'  
```

`ffmpeg -f concat -i list.txt -c copy outputFile.webm`

More methods: <https://stackoverflow.com/a/11175851>
