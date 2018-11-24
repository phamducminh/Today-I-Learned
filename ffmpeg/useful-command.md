# FFMpeg Useful Commands

> This note is an list of useful command when I've been working with FFmpeg

To be continue...

---

* List all packet of media source

```
ffprobe -show_packets <path_to_source>
```

* Show video profile

```
ffprobe -v error -select_streams v:0 -show_entries stream=profile,level -of default=noprint_wrappers=1
```

You can list more info of video stream, just put them in stream param, separate by comma.

Example: **stream=profile,level,width,height,avg_frame_rate,duration,bit_rate**. This will list h264 profile, h264 level, width,
height, frame rate, bit rate of a video.
