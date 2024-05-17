---
layout: post
title: "Extracting High-Quality Keyframes from Videos Using FFmpeg"
tags: Tutorial FFmpeg
---

I recently found out that when extracting individual frames from a video, not all frames are of the same quality. This is because [temporal compression](https://en.wikipedia.org/wiki/Video_compression_picture_types) is employed to store videos in a more efficient way, without needing to store each individual frame in its full size. It exploits the similarities between neighboring frames to reduce the amount of data needed to store the video. This can lead to artifacts such as blurriness when frames are extracted as individual images.

FFmpeg has a way to extract only high quality frames from a video, by extracting **keyframes**: Keyframes are stored as complete images within the video stream and should have higher quality than intermediate frames. There are two ways of extracting keyframes in ffmpeg:

1. Using the `-skip_frame nokey` option:

    ```bash
    ffmpeg -skip_frame nokey -i input.mp4 -vsync vfr -frame_pts true out_%03d.png
    ```

    - `skip_frame nokey` skips all non-keyframes
    - `vsync vfr` discards unused frames to avoid duplicates
    - `frame_pts true` uses the frame index for naming the output images

2. Using the `-vf "select=eq(pict_type,I)"` filter:
    
    ```bash
    ffmpeg -i input.mp4 -vf "select=eq(pict_type\,I)" -vsync vfr out_%03d.png
    ```

    - `select='eq(pict_type,I)'` selects only frames where the picture type is I (keyframe)

I prefer option 1, because the resulting frame names contain the exact position of the frame within the video (because of the `-frame_pts true` flag). 
