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

| Signature | Java Type |
| ------| ------ |
| Z | boolean |
| B | byte |
| C | char |
| S | short |
| I | int |
| J | long |
| F | float |
| D | double |
| L fully-qualified-class ; | fully-qualified-class |
| [ type | type[] |
| ( arg-types ) ret-type | method type |

#### Reference:
* [Mac 基础教程：如何让 Finder 显示隐藏文件和文件夹](https://sspai.com/post/26273)