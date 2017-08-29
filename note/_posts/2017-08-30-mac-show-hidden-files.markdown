---
layout: post
title: Show hidden files on macOS
---

#### Show hidden files:
{% highlight bash %}
$ defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder
{% endhighlight %}

#### Hide hidden files:
{% highlight bash %}
$ defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder
{% endhighlight %}

#### Reference:
* [Mac 基础教程：如何让 Finder 显示隐藏文件和文件夹](https://sspai.com/post/26273)