---
title: FFmpeg Export Manual
---

# FFmpeg Export Manual

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Video -> Audio](#video---audio)
- [Encode with ACC](#encode-with-acc)
- [Trim](#trim)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

> Reference: [FFmpeg documents](https://ffmpeg.org/ffmpeg.html#toc-Main-options)

## Video -> Audio 

```sh
ffmpeg -i "<video.file>" -vn -acodec copy "<exported audio.file>"  
```

## Encode with ACC

>  Reference: https://trac.ffmpeg.org/wiki/Encode/AAC

```sh
ffmpeg -i "<audio.file>" -c:a aac "<audio.file>.m4a"
```

## Trim

> Reference: [Time duration](https://ffmpeg.org/ffmpeg-utils.html#time-duration-syntax)

```sh
# -ss start time / -to end time
ffmpeg -i "<audio.file>" -c:a aac -ss 00:00:00 -to 00:02:37 "<audio.file>.m4a"
```

