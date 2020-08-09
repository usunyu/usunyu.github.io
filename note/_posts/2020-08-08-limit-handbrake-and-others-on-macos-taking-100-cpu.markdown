---
layout: post
title: Limit HandBrake and others from taking 100% CPU
---

For macOS, Download [cputhrottle](http://www.willnolan.com/cputhrottle/cputhrottle.html)

Unzip it, then in a terminal, make it executable:
```
chmod +x ./cputhrottle
```

![](../../../../../public/images/limit_handbrake/1.png)

Make sure open `Security & Privacy` to allow the app.

```
sudo cputhrottle 46240 100
```

To avoid having to manually lookup the PID, and to run the cputhrottle process in the background, use the following:

```
sudo cputhrottle $(pgrep -f HandBrakeXPCService) 100 &
```

Stop cputhrottle:

```
sudo pkill cputhrottle
```

Update:

Use [CPUSetter](https://www.whatroute.net/cpusetter.html) instead, don't forget to donate the author :)

#### Reference:
* [Limit Dropbox and others on macOS from taking 100% CPU](https://medium.com/@sbr464/limit-dropbox-and-others-on-macos-from-taking-100-cpu-877266df104d)