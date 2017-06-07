---
layout: post
title: Reduce .git folder size
---

#### To see the 10 biggest files, run this from the root directory:

{% highlight bash %}
$ git verify-pack -v .git/objects/pack/pack-7b03cc896f31b2441f3a791ef760bd28495697e6.idx \
| sort -k 3 -n \
| tail -10
{% endhighlight %}

#### To see what each file is, run this:

{% highlight bash %}
$ git rev-list --objects --all | grep [first few chars of the sha1 from previous output]
{% endhighlight %}

#### Rewrite all the commits:

{% highlight bash %}
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch "Folder Name/*"' -- --all
$ rm -Rf .git/refs/original
$ rm -Rf .git/logs/
$ git gc --aggressive --prune=now
{% endhighlight %}

#### Then verify:

{% highlight bash %}
$ git count-objects -v
{% endhighlight %}

#### Reference:
* [Consider cleaning up the .git folder to reduce the large repo size](https://github.com/18F/C2/issues/439)
* [Git Internals - Maintenance and Data Recovery](https://git-scm.com/book/en/v2/Git-Internals-Maintenance-and-Data-Recovery)