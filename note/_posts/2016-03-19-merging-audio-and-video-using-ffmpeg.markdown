---
layout: post
title:  "Merging audio and video using ffmpeg"
---

### Download and install [ffmpeg](http://www.ffmpeg.org/download.html)

### Open a Command Prompt window in your downloads folder and run the following command
##### 1. In case of MP4 format (all, except 1440p 60fps & 2160p 60fps):
{% highlight bash %}
$ ffmpeg -i videoplayback.mp4 -i videoplayback.m4a -c:v copy -c:a copy output.mp4
{% endhighlight %}

##### 2. In case of WebM format (1440p 60fps and 2160p 60fps):
{% highlight bash %}
$ ffmpeg -i videoplayback.webm -i videoplayback.m4a -c:v copy -c:a copy output.mkv
{% endhighlight %}

### Wait until ffmpeg finishes merging audio and video into a single file named "output.mp4"