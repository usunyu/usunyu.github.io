---
layout: post
title:  "Installing ffmpeg with libx264 (h264) on MAC OS X"
---

#### Install libx264:
Get the libx264 package from [videolan x264](http://www.videolan.org/developers/x264.html)

{% highlight bash %}
$ tar -xjvf last_x264.tar.bz2
$ cd last_x264
$ ./configure --enable-shared
$ make
$ sudo make install
{% endhighlight %}

#### Install ffmpeg:
From [ffmpeg](http://www.ffmpeg.org/download.html) find the latest stable release

{% highlight bash %}
$ tar xvfj ffmpeg-3.0.2.tar.bz2
$ cd ffmpeg-3.0.2
$ ./configure --enable-libx264 --enable-gpl --enable-shared
$ make
$ sudo make install
{% endhighlight %}