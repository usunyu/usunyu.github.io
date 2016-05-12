---
layout: post
title:  "Git Bisect"
---

{% highlight bash %}
$ git bisect reset
$ git bisect start
$ git bisect bad
$ git bisect good <commit>
Bisecting: 10 revisions left to test after this (roughly 3 steps)
...
{% endhighlight %}