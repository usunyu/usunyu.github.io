---
layout: post
title:  "ADB Shell Command"
---

{% highlight bash %}
$ adb devices
$ // take screenshot
$ adb shell screencap -p /sdcard/screencap.png
$ adb pull /sdcard/screencap.png
$ // transfer file
$ adb push filename.zip /sdcard/
$ // pull apk
$ adb shell pm list packages
$ adb shell pm path com.example.package
$ adb pull <path_returned>
{% endhighlight %}