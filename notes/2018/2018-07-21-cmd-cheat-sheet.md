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

### Batch To Mp3.bat
Loop through every video in this directory and convert it to V2 .mp3.
```
for %%a in ("*.mkv" "*.mp4" "*.webm") do ffmpeg -i "%%a" -codec:a libmp3lame -qscale:a 2 "%%a.mp3"
```

### Encode VP8 Webm
```
-c:v libvpx -pix_fmt yuv420p -lag-in-frames 16 -auto-alt-ref 1 -qmin 28 -crf 30 -qmax 32 -qcomp 1 -b:v 0
```

### Convert to JPEG
Convert all BMP files in directory to JPEG then delete them.
```
magick mogrify -format jpg -quality 80 *.bmp
del *.bmp
```

## Image Editing

* `convert` creates a new file  
* `mogrify` overwrites the original file

### Resize

```
magick convert "thing.jpg" -resize 50% "thing-small.jpg"
magick mogrify "thing.jpg" -resize 50%
```

## Video Editing

### Hold the last frame

`N` in `trim=0:N` changes the duration of the freeze.
```
ffmpeg -i in.webm -filter_complex "[0]trim=0:N[hold];[0][hold]concat[extended];[extended][0]overlay" -c:v libvpx -pix_fmt yuv420p -lag-in-frames 16 -auto-alt-ref 1 -qmin 28 -crf 30 -qmax 32 -qcomp 1 -b:v 0 out.webm
```