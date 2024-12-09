---
title: "Reducing Video Resolution with FFmpeg"
categories:
  - Blog
tags:
  - Programming
  - FFmpeg
  - Video
---

The other day I had an [`.mkv` file](https://en.wikipedia.org/wiki/Matroska) that I needed a short clip from in [MP4](https://en.wikipedia.org/wiki/MP4_file_format) format. The original file was in 1080p, but since the audio was the more important part of the video, I didn't need to keep it in such high resolution. I used [FFmpeg](https://en.wikipedia.org/wiki/FFmpeg) for the file conversion.

```bash
ffmpeg -i input.mkv -codec copy step1.mp4
```

I trimmed the video to the desired section using [QuickTime Player](https://en.wikipedia.org/wiki/QuickTime#QuickTime_Player). I used FFmpeg again to reduced the resolution.

```bash
ffmpeg -i step1-trimmed.mp4 -vf scale=640:360 output.mp4
```

There's a lot more to effective video compression, but I just wanted to have this written down in case I ever need it again.
