---
layout: post
title: Sublime Tips
---

### Terminal Setup for Mac:
{% highlight bash %}
$ ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl
{% endhighlight %}


### Support Chinese:
###### ctrl + `, open console and input:
{% highlight bash %}
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
{% endhighlight %}

###### Restart Sublime

###### Ctrl + Shift + P -> Install Package

###### Install "ConvertToUTF8" or "GBK Encoding Support"


### Key Binding Setup for Windows:
{% highlight bash %}
[
    { "keys": ["ctrl+1"], "command": "select_by_index", "args": { "index": 0 } },
    { "keys": ["ctrl+2"], "command": "select_by_index", "args": { "index": 1 } },
    { "keys": ["ctrl+3"], "command": "select_by_index", "args": { "index": 2 } },
    { "keys": ["ctrl+4"], "command": "select_by_index", "args": { "index": 3 } },
    { "keys": ["ctrl+5"], "command": "select_by_index", "args": { "index": 4 } },
    { "keys": ["ctrl+6"], "command": "select_by_index", "args": { "index": 5 } },
    { "keys": ["ctrl+7"], "command": "select_by_index", "args": { "index": 6 } },
    { "keys": ["ctrl+8"], "command": "select_by_index", "args": { "index": 7 } },
    { "keys": ["ctrl+9"], "command": "select_by_index", "args": { "index": 8 } },
    { "keys": ["ctrl+0"], "command": "select_by_index", "args": { "index": 9 } },
    { "keys": ["ctrl+shift+g"], "command": "find_all_under" },
]
{% endhighlight %}


#### Reference:
* [如何解决Sublime Text 3不能正确显示中文的问题](https://segmentfault.com/a/1190000002461891)