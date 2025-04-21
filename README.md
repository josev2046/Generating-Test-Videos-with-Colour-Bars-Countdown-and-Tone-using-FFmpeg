This FFmpeg command generates a test video with SMPTE colour bars, a "myVideo" text overlay, a countdown timer, and a simple sine wave audio track - useful for creating basic test video files with an accompanying audio tone for various testing and calibration purposes.

![image](https://github.com/user-attachments/assets/64767e63-12df-4a41-b1d4-afd4e440e265)

~~~```bash
ffmpeg -f lavfi \
       -i testsrc=duration=180:size=1920x1080:rate=30 \
       -f lavfi \
       -i "sine=frequency=440:duration=180" \
       -vf "drawtext=fontfile=/path/to/font.ttf:text='myVideo':fontcolor=black:fontsize=100:x=(w-text_w)/2:y=(h-text_h)/2-50,drawtext=fontfile=/path/to/font.ttf:text='%{eif\\:trunc((180-t)/60)\\:d}\\:%{eif\\:mod(180-t\\,60)\\:d\\:2}':fontcolor=black:fontsize=72:x=(w-text_w)/2:y=(h-text_h)/2+50,format=yuv420p" \
       -c:v libx264 \
       -crf 18 \
       -preset veryfast \
       -c:a aac \
       -b:a 128k \
       -map 0:v \
       -map 1:a \
       -t 180 \
       myVideo_with_tone.mp4
~~~

**Breakdown:**

*  **Video Generation (`-f lavfi -i testsrc=...`):** Creates the visual part of the video, including colour bars and the text overlays.
*  **Audio Generation (`-f lavfi -i "sine=frequency=440:duration=180"`):** Generates a basic 440Hz sine wave as the soundtrack for the duration of the video.
*  **Video Encoding (`-c:v libx264 ...`):** Encodes the video stream using the H.264 codec for good quality and compatibility.
*  **Audio Encoding (`-c:a aac ...`):** Encodes the audio stream using the AAC codec, a common format for audio.
*  **Stream Mapping (`-map 0:v -map 1:a`):** Tells FFmpeg which input streams to use for video (stream 0) and audio (stream 1).
*  **Output File (`myVideo_with_tone.mp4`):** Specifies the name of the resulting video file with both video and audio.

