# FFmpeg Video and Audio Processing Script

This script utilizes FFmpeg to perform a variety of video and audio processing tasks, making it an essential tool for security footage analysis, podcast and vlog development, and automation. By leveraging this script, users can quickly filter videos, extract or merge audio, overlay images, and much more, streamlining their multimedia content creation and analysis workflows.

## Features

1. **Quick Video Filter**: Apply various filters to a video file.
   ```bash
   ffmpeg -i "$1" -vf "$2" "$3"
   ```

2. **Extract Audio**: Remove video and keep the sound as an MP3 file.
   ```bash
   ffmpeg -i "$1" -vn "${1%.*}".mp3
   ```

3. **Extract Video**: Remove sound and keep the video as an MP4 file.
   ```bash
   ffmpeg -i "$1" -an "${1%.*}".mp4
   ```

4. **Clip Video**: Grab a portion of a video and save it as an MP4 file.
   ```bash
   ffmpeg -i "$1" -t "$2" "${1%.*}"_portion.mp4
   ```

5. **Add Image to Audio**: Combine a picture and audio to create an MP4 video.
   ```bash
   ffmpeg -loop 1 -i "$1" -i "$2" -c:v libx264 -c:a aac -strict experimental -b:a 192k -shortest "${2%.*}"_musicstill.mp4
   ```

6. **Overlay Logo**: Add a logo to a video and save it as an MP4 file.
   ```bash
   ffmpeg -y -i "$1" -i "$2" -filter_complex "overlay=x=main_w-overlay_w-(main_w*0.01):y=main_h-overlay_h-(main_h*0.01)" "${1%.*}"_withlogo.mp4
   ```

7. **Add Sound Waves**: Add sound waves visualization to a picture or video.
   ```bash
   ffmpeg -y -i "$1" -loop 1 -i "$2" -filter_complex "[0:a]showwaves=s=1280x720:mode=line,colorkey=0x000000:0.01:0.1,format=yuva420p[v];[1:v][v]overlay[outv]" -map "[outv]" -pix_fmt yuv420p -map 0:a -c:v libx264 -c:a copy -shortest "${2%.*}"_soundwave.mp4
   ```

8. **Segment Video**: Cut a video into segments.
   ```bash
   ffmpeg -i "$1" -map 0 -c copy -f segment -segment_time 1800 "${1%.*}"_%03d.mp4
   ```

9. **Add Background Music**: Merge background music with an audio file.
   ```bash
   ffmpeg -i "$1" -i bgmusic.mp3 -filter_complex amerge=inputs=2 -ac 2 -shortest "${1%.*}"_backgroundmusic.mp3
   ```

10. **Chipmunk Voice**: Alter the audio to create a chipmunk voice effect.
    ```bash
    ffmpeg -i "$1" -af asetrate=44100*1.3,aresample=44100 "${1%.*}"_chipmunk.mp3
    ```

11. **Sound Waves to GIF**: Add sound waves to a song and convert a GIF to an MP4.
    ```bash
    ffmpeg -y -i "$1" -ignore_loop 0 -t 28800 -i "$2" -filter_complex "[0:a]showwaves=s=400x240:mode=line,colorkey=0x000000:0.01:0.1,format=yuva420p[v];[1:v][v]overlay[outv]" -map "[outv]" -pix_fmt yuv420p -map 0:a -c:v libx264 -c:a copy -shortest "${1%.*}"_song_gif.mp4
    ```

## Usage

1. **Quick Video Filter**: Apply a filter to a video file.
   ```bash
   ./script.sh input.mp4 filter "output.mp4"
   ```

2. **Remove Video, Keep Sound**: Extract audio from a video file.
   ```bash
   ./script.sh input.mp4
   ```

3. **Remove Sound, Keep Video**: Extract video from a file.
   ```bash
   ./script.sh input.mp4
   ```

4. **Grab a Portion of a Video**: Extract a segment of a video.
   ```bash
   ./script.sh input.mp4 00:01:00
   ```

5. **Add Picture to Audio**: Combine an image and audio to create a video.
   ```bash
   ./script.sh image.png audio.mp3
   ```

6. **Add Logo to Video**: Overlay a logo on a video.
   ```bash
   ./script.sh video.mp4 logo.png
   ```

7. **Add Sound Waves to Picture/Video**: Visualize sound waves on media.
   ```bash
   ./script.sh audio.mp3 image.png
   ```

8. **Cut Video into Segments**: Divide a video into smaller parts.
   ```bash
   ./script.sh video.mp4
   ```

9. **Add Background Music**: Combine background music with an audio file.
   ```bash
   ./script.sh audio.mp3
   ```

10. **Chipmunk Voice**: Apply a chipmunk effect to an audio file.
    ```bash
    ./script.sh audio.mp3
    ```

11. **Sound Waves to GIF**: Add sound waves to a song and convert GIF to MP4.
    ```bash
    ./script.sh audio.mp3 animation.gif
    ```

## Applications

- **Security Footage Analysis**: Quickly filter, clip, and process video files for reviewing security footage.
- **Podcast Development**: Extract audio, add background music, and apply audio effects for creating engaging podcasts.
- **Vlog Development**: Combine video clips, overlay logos, and enhance videos with visual and audio effects for high-quality vlogs.
- **Automation**: Automate repetitive multimedia processing tasks to save time and improve efficiency in content creation.

## License

This project is licensed under the MIT License.

## Acknowledgements

- FFmpeg: The powerful multimedia framework used for processing videos and audios.