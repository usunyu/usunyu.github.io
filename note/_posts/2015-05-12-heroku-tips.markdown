---
layout: post
title:  "Heroku Command Tips"
---
#### Add Vim:
{% highlight bash %}
$ heroku plugins:install https://github.com/naaman/heroku-vim
$ heroku vim
{% endhighlight %}

#### Keep Heroku Alive:
##### 1. Installing the Scheduler add-on:
{% highlight bash %}
$ heroku addons:create scheduler:standard
{% endhighlight %}

##### 2. Defining tasks `app/bin/ping_server.py`:
{% highlight python %}
import urllib2

try:
    urllib2.urlopen("https://sleepy-scrubland-5678.herokuapp.com/")
    print "URL Exist"
except ValueError, ex:
    print "URL not well formatted"
except urllib2.URLError, ex:
    print "URL don't seem to be alive"
{% endhighlight %}

##### 3. Testing tasks:
{% highlight bash %}
$ heroku run python bin/ping_server.py
{% endhighlight %}

##### 4. Scheduling jobs:
{% highlight bash %}
$ heroku addons:open scheduler
{% endhighlight %}

#### Resource:
* [Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler)
* [python-verify-url-goes-to-a-page](http://stackoverflow.com/questions/4041443/python-verify-url-goes-to-a-page)
* [what-text-editor-is-available-in-heroku-bash-shell](http://stackoverflow.com/questions/12666799/what-text-editor-is-available-in-heroku-bash-shell)