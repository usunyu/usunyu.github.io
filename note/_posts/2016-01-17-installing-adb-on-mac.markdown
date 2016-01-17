---
layout: post
title:  "Installing ADB on MAC OS X"
---

### Using Homebrew:
This is the easiest way and will provide automatic updates.
##### 1. Install [homebrew](http://brew.sh/):
{% highlight bash %}
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

##### 2. Install adb:
{% highlight bash %}
$ brew install android-platform-tools
{% endhighlight %}

##### 3. Start using adb:
{% highlight python %}
$ adb devices
{% endhighlight %}

#### Resource:
* [installing-adb-on-mac-os-x](http://stackoverflow.com/questions/31374085/installing-adb-on-mac-os-x)