---
title: Offline streaming with OBS
tags:
  - OBS
  - offline
  - stream
---

Whether you're hosting a local event like or setting up an offline streaming environment, sharing your content in real time can be both efficient and effective. OBS (Open Broadcaster Software) is a popular choice for broadcasting, but it functions only as a client. To stream locally without relying on external platforms, you'll need a server that supports RTSP (Real-Time Streaming Protocol)—the protocol OBS uses to publish streams.

That’s where [MediaMTX](https://mediamtx.org) comes in.

##  How to Set It Up

### 1. Configure OBS for Local Streaming

In OBS, go to:

```
Settings > Stream > Custom
```

Enter the following URL:

```
rtmp://localhost/live
```

You'll also need to set a stream key.

### 2. Run MediaMTX with Docker

Use the following command to launch MediaMTX locally:

```bash
sudo docker run --rm -it \
-e MTX_RTSPTRANSPORTS=tcp \
-p 8554:8554 \
-p 1935:1935 \
-p 8888:8888 \
-p 8889:8889 \
-p 8890:8890/udp \
-p 8189:8189/udp \
bluenviron/mediamtx:1-ffmpeg
```

This sets up MediaMTX to handle RTSP and RTMP streams and makes it accessible via WebRTC.

### 3. View the Stream in a Browser

Anyone on the local network can view the stream using:

```
http://localhost:8889/live/{stream-key}/
```

Just replace `{stream-key}` with the one you set in OBS.

## Final Tweaks

To ensure smooth playback with minimal requirements, you may need to adjust encoding settings in OBS. Lowering resolution and bitrate can help make the stream more accessible across devices.