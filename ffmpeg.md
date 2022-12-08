# ffmpeg

ffmpeg is a mighty tool for dealing with videos. Around the ffmpeg core is a rich ecosystem with video-editing related tools. First, we look at some handy examples to get a feeling how to deal with ffmpeg.  

## Converting different file formats
Getting basic information about a video is as easy as:
```
ffmpeg -i input.avi
```
Converting an mp4-video to an avi-video with ffmpeg:
```
ffmpeg -i input.mp4 output.avi
```
The quality of the output can be defined as well with a number between 1 (high quality) and 50 (low quality). If you output-video is an avi you use the -p parameter. If your output-video is mp4 you use the parameter -crf instead. Have a look at the exmaple:
```
ffmpeg -i input.mp4 -q 30 output.avi
ffmpeg -i input.avi -crf 30 output.mp4
```
Use ffmpeg to convert an input-video to an output-video and set the framerate (parameter -r) of the output video to 5 frames:
```
ffmpeg -i input.mp4 -r 5 output.avi
```
Scaling a video can be done with ffmpeg as well. Use the scale video filter. This filter has two parameters, notice that the second one is -1, this means that the scaling for the y-axis is proportional.
```
ffmpeg -i input5mb-1280x720.mp4 -vf scale=300:-1 out.mp4
```
 
## Changing colors by using ffmpeg video filters
Convert input video to a greyscale output video with ffmpeg (see the -vf videofilter parameter):
```
ffmpeg -i input.mp4 -vf colorchannelmixer=.3:.4:.3:0:.3:.4:.3:0:.3:.4:.3 out.avi
```
Convert input video to a sepia output video with ffmpeg:
```
ffmpeg -i input.mp4 -r 15 -vf colorchannelmixer=.393:.769:.189:0:.349:.686:.168:0:.272:.534:.131 out.avi
```
 
## Joining (concatenate) and cutting videos
Cutting video with ffmpeg is simple. You want to specify a start and end time to convert only a certain timeslice? No Problem. The parameter -ss 30:15 let the video start at 30 minutes and 15 seconds. The parameter -t 40 defines the length. Take care of the parameters orders. This example cuts a 40-second lasting piece and starts at 30 min and 15 seconds:
ffmpeg -ss 30:15 -i input.mp4 -t 40 output.mp4
You can even join (concatenate) videos together with ffmpeg. Concat two avi-videos together can be done with this command:
```
ffmpeg -i "concat:part1.avi|part2.avi" -codec copy output.avi
```
Joining mp4-videos doesn't work the before mentioned way. Here we first create some intermediate ts files and join them afterwards:
```
ffmpeg -i in1.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts tmp1.ts
ffmpeg -i in2.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts tmp2.ts
ffmpeg -i in3.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts tmp3.ts
ffmpeg -i "concat:tmp1.ts|tmp2.ts|tmp3.ts" -c copy -bsf:a aac_adtstoasc result.mp4
```

## Caturing video from webcam with ffmpeg
Capturing video from a webcam with ffmpeg and store it in output-video.
(It is important to know the name of your webcam device. In my case, it is /dev/video0 under linux. You can research this with the command: ffmpeg -sources)
```
ffmpeg -i /dev/video0 out.avi
```

## Basic streaming
```
ffmpeg -re -i movie01.mp4 -f mpegts udp://127.0.0.1:23000
ffmpeg -i /dev/video0 -f mpegts udp://127.0.0.1:23000
```

### stream video with optimized high quality
```
ffmpeg -re -i v720x576.avi -c:v libx264 -f mpegts udp://127.0.0.1:23000
```

-re keeps the input and output at the same playback rate ()
-i defines the input device or file

consume this stream with ffplay
```
ffplay udp://127.0.0.1:23000
ffplay -fflags nobuffer -framedrop udp://127.0.0.1:23000
```

consume this stream with vlc
```
udp://@:23000
```

list supported devices for v4l2
```
ffplay -f video4linux2 -list_formats all /dev/video0
```

grab the framerate of an device

```
ffplay -f v4l2 -framerate 60 /dev/video0
```
