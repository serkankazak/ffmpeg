## ffmpeg

install from
```
https://www.gyan.dev/ffmpeg/builds/ffmpeg-git-full.7z
```

record screen
```
ffmpeg.exe -f gdigrab -framerate 30 -i desktop screen.mkv
```
```
ffmpeg.exe -f gdigrab -framerate 25 -c:v libx264 -g 50 -b:v 4000k -bufsize 8000k -vf format=yuv420p -preset fast -i desktop screen25.mkv
```

blur at 540, 460 with size 200, 75
```
ffmpeg.exe -i in.mp4 -filter_complex "[0:v]crop=200:75:540:460,avgblur=15[fg];[0:v][fg]overlay=540:460[v]" -map "[v]" -c:v libx264 -preset slow -crf 0 -movflags +faststart blur.mp4
```

compress all videos in folder
```
while read -r c; do d=$(echo "$c" | sed 's/\..*$//' | awk '{print "c_"$0".mp4"}'); echo "$d"; if [ ! -e "$d" ]; then echo "$c"; ./ffmpeg.exe -nostdin -v error -i "$c" -c:v libx264 -crf 23 -c:a aac "$d" 2> /dev/stdout < /dev/null; fi; done < <(find . -type f -not -iname "ffmpeg*" | sed 's/\.\///' | grep -vE "^c_")
```

concat
```
(for %i in (*.mp4) do @echo file '%i') > go.txt
ffmpeg -safe 0 -f concat -i go.txt -c:v libx264 out.mp4
```

cut
```
ffmpeg.exe -ss 00:00:30.0 -to 06:32:23.0 -i in.mkv -c:v libx264 cut.mkv
```
