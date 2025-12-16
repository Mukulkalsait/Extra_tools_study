##### YOUTUBE:Best quality video creation.

```bash
ffmpeg -i VID_20250919_201136.mp4 \
-c:v libx264 -preset slow -crf 18 -pix_fmt yuv420p -r 60 \
-vf "transpose=1" \
-c:a aac -b:a 320k output_youtube.mp4
```

Explanation:

    -c:v libx264 → re-encode to H.264 (more compatible).
    -preset slow -crf 18 → high quality, reasonable size.
    -pix_fmt yuv420p → standard format YouTube likes.
    -r 60 → force stable 60 fps.
    -vf "transpose=1" → rotate -90° to make it upright. (If it comes out wrong, use transpose=2).
    -c:a aac -b:a 320k → good audio quality.
    👉 After this, you’ll get a YouTube-ready MP4 that will upload smoothly.

#### NVIDIA + YOUTUBE:Best quality video creation.

```bash
nvidia-offload ffmpeg -i input.mp4 \
  -c:v h264_nvenc -preset slow -b:v 20M \
  -c:a aac -b:a 320k -ar 48000 \
  -movflags +faststart output.mp4
```

Explanation:

      -nvidia-offload → runs ffmpeg on NVIDIA GPU.
      -c:v h264_nvenc → use NVIDIA’s H.264 hardware encoder.
      -preset slow → better compression (you can try medium for faster).
      -b:v 20M → video bitrate (20 Mbps is good for 4K 60fps YouTube).
      -c:a aac -b:a 320k -ar 48000 → high quality stereo audio.
      -movflags +faststart → optimizes file for streaming/uploading.
