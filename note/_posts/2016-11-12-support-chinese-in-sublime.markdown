---
layout: post
title:  "Support Chinese for Sublime Text"
---

#### ctrl + `, open console and input:
{% highlight bash %}
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
{% endhighlight %}

#### Restart Sublime

#### Ctrl + Shift + P -> Install Package

#### Install "ConvertToUTF8" or "GBK Encoding Support"

#### Reference:
* [如何解决Sublime Text 3不能正确显示中文的问题](https://segmentfault.com/a/1190000002461891)