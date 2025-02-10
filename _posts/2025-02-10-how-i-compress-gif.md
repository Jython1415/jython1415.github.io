---
title: "How I create compressed GIFs of screen recordings"
categories:
  - Blog
tags:
  - Something I Learned
  - Media
  - FFmpeg
  - Tech
  - Video
---

The other day I needed to upload a screen recording to report a bug[^1]. Here's how I produced a compressed GIF.

1. Record the screen with the macOS built-in screen capture: ⌘⇧5
2. Rename/copy the screen recording to `input.mov`
3. `ffmpeg -i input.mov -vf "scale=1470:956,fps=10" -pix_fmt rgb8 output1.gif`
4. `gifsicle -O3 output1.gif -o output2.gif`

The output doesn't look great but you can still tell what's going on. Most importantly, the 20–25 second capture was reduced to ~2 MB.

In the future, I can improve this method by making the scaling more flexible. For example, it would be nice if it could scale to be 2x smaller, regardless of the original aspect ratio.

[^1]: [Vim cursor disappears when following an internal link to a new tab - Bug reports - Obsidian Forum](https://forum.obsidian.md/t/vim-cursor-disappears-when-following-an-internal-link-to-a-new-tab/96024)
