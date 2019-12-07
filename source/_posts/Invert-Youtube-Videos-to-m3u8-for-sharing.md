---
title: Invert Youtube Videos to m3u8 for sharing
date: 2019-12-07 12:41:39
tags: tech
---

# Invert Youtube Video to HLS(m3u8) on macOS



## DOWNLOAD

```
youtube-dl -f 22 DOWNLOAD_URL
```

# INVERT

1. Invert to mp4 

   ```
   ffmpeg -i input.mkv -acodec copy -vcodec copy out.mp4 
   ```

   

2. Invert to ts

   ```
   ffmpeg -i out.mp4 -c copy -bsf h264_mp4toannexb output.ts 
   ```

   or

   ```
   ffmpeg -i out.mp4 -c copy  output.ts 
   ```

   

3. Segment ts

   ```
   ffmpeg -i output.ts -c copy -map 0 -f segment -segment_list playlist.m3u8 -segment_time 5 output%03d.ts  
   ```

   