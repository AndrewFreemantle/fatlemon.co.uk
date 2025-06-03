---
title: How to download a YouTube channel
permalink: /2025/03/how-to-download-a-youtube-channel/
excerpt: Sync a YouTube channel to your media server or your local computer
tags:
  - YouTube
  - download
  - docker
---

![alt text here]({{ site.imageurl }}2025/youtube-sync/youtube-sync-header.jpg)
<figcaption>Image by <a href="https://www.freepik.com/free-photo/pile-3d-play-button-logos_1191373.htm">natanaelginting on Freepik</a></figcaption>

If your media server supports [Docker](https://www.docker.com) containers, and has a way to [schedule scripts](https://en.wikipedia.org/wiki/Cron) then the following script run daily (or as often as you like) will sync a YouTube channel:

``` bash
#!/bin/bash

docker run --rm \
  -v "/path/to/media/TV Shows/{channel}/:/downloads/:rw" \
  -v "/path/to/application-data-directory/youtube-dl/cache/:/cache/:rw" \
  -v "/path/to/application-data-directory/youtube-dl/tmp/:/tmp/:rw" \
  --cpus 2 \
  jauderho/yt-dlp:latest \
    --playlist-items 1:5 \    # only download the most recent 5 videos (remove this for the first sync)
    --add-metadata \
    --convert-thumbnails png \
    --download-archive /cache/archive.txt \
    --embed-chapters \
    --embed-subs \
    --embed-thumbnail \
    --merge-output-format mp4 \
    --remux-video mp4 \
    --no-overwrites \
    --output "%(channel)s - %(timestamp>%Y)s-%(timestamp>%m)s-%(timestamp>%d)s - %(title)s.%(ext)s" \
    --paths "/downloads/" \
    --paths "temp:/tmp/" \
    --write-description \
    --write-info-json \
    --write-annotations \
    "https://www.youtube.com/@{channel}/videos"
```
