+++
title = "FFmpeg Cheat Sheet"
slug = "ffmpeg-cheet-sheet"
date = 2018-05-19T04:07:28+01:00
tags = ["media", "ffmpeg", "video", "batch", "audio"]
categories = ["Media"]
draft = false
+++
<!--more-->

## Conversion

### BatchToMp3.bat
Loop through every video in this directory and convert it to V2 .mp3.
```
for %%a in ("*.mkv" "*.mp4" "*.webm") do ffmpeg -i "%%a" -codec:a libmp3lame -qscale:a 2 "%%a.mp3"
```

### Encode VP8 Webm
```
-c:v libvpx -pix_fmt yuv420p -lag-in-frames 16 -auto-alt-ref 1 -qmin 28 -crf 30 -qmax 32 -qcomp 1 -b:v 0
```

## Video Editing

### Hold the last frame

`N` in `trim=0:N` changes the duration of the freeze.
```
ffmpeg -i in.webm -filter_complex "[0]trim=0:N[hold];[0][hold]concat[extended];[extended][0]overlay" -c:v libvpx -pix_fmt yuv420p -lag-in-frames 16 -auto-alt-ref 1 -qmin 28 -crf 30 -qmax 32 -qcomp 1 -b:v 0 out.webm
```