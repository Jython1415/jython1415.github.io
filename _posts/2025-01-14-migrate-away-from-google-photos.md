---
title: "Migrating Away from Google Photos"
categories:
  - Blog
tags:
  - Tech
  - Media
---

It finally happened—I ran out of space in my Google account. Actually, it happened two weeks ago, but it took 2 weeks of seeing the "You will no longer receive new emails" for the urgency to kick in. As you might expect, most of the free 15 GB of storage space was consumed by Google Photos (I switched to it when they were still offering "unlimited storage" if you allowed them to compress photos), so this post a record of how I migrated away from Google Photos.

Since the total space taken up by Google Photos was < 15 GB, I decided to go with a local storage, on my MacBook Air and backed up on an external SSD as well.

The export step was straightforward. Google Takeout allows for easy exporting of any Google-related data, so I used it to export all my photos. The first export I requested was with 2 GB splits, but the resulting export had 25 files which seemed a bit much. After re-requesting with 10 GB splits, I proceeded with downloading.

The download would have been smoother had I been on a better network connection. For one, I was back in Tokyo during this process, and my dad was working through some issues with the home WiFi setup as the same time. On top of that, I was on a VPN connection because some sites wouldn't load otherwise. Combined, these factors made the download challenging. I ended up getting a stable connection with around ~2 MB/s download, but the download would pause every time the connection got interrupted, so it took a lot of monitoring for all the downloads to complete.

The import into the Photos app went quicker than I expected. First, I was just dragging the top-level folder from the export into the app, but it worked better when I used the "Import" option from the menu bar (Cmd + Shift + I) because it makes the option for importing *just* the new items a lot clearer.

And finally, I had to delete everything from Google Photos, which was the worst part of the process. If I was deleting *all* my Google-related data it would be a lot simpler, but I had to go through and manually select all the photos I wanted to delete.

I thought I could click the first photo, click the scroll bar on the side to get to the bottom, and then shift click the bottom photo to select everything for deletion. Unfortunately, this didn't work. It might have worked if I manually scrolled all the way to the bottom instead of using the scroll bar on the side to get to the bottom. Someone else will have to test this because I no longer have photos in Google Photos anymore.

Here's my photo management solution now:

- Primary storage: Photos.app on MacBook Air.
- Backup: 2 TB external SSD until I can back it up on my 12 TB HDD back in Cleveland.
- Photo import: Direct from iPhone via cable
- Photo sharing: I never used link sharing from Google Photos, so no change—I just send the photos directly.

