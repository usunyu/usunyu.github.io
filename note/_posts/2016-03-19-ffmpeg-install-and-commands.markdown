---
layout: post
title:  "FFmpeg install and useful commands"
---

### Download and install [ffmpeg](http://www.ffmpeg.org/download.html)

### Merging audio and video

##### 1. In case of MP4 format (all, except 1440p 60fps & 2160p 60fps):
{% highlight bash %}
$ ffmpeg -i video.mp4 -i audio.m4a -c:v copy -c:a copy output.mp4
{% endhighlight %}

##### 2. In case of WebM format (1440p 60fps and 2160p 60fps):
{% highlight bash %}
$ ffmpeg -i videoplayback.webm -i videoplayback.m4a -c:v copy -c:a copy output.mkv
{% endhighlight %}

### Split video with specific timestamp
{% highlight bash %}
$ ffmpeg -i output.mp4 -ss 0 -t 60 first-60-sec.mp4
{% endhighlight %}

### Replace old audio stream with target audio stream
{% highlight bash %}
$ ffmpeg -i video.mp4 -i audio.mp3 -c:v copy -c:a aac -strict experimental -map 0:v:0 -map 1:a:0 output.mp4
{% endhighlight %}