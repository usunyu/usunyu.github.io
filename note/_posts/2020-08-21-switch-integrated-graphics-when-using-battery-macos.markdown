---
layout: post
title: Switch to integrated graphics when using battery on macOS
---

Run them one after the other. Each command sets a different system flag. They’re persistent once set. However, if you go into power options in Preferences and set the flag to autoswitch, it appears that these are reset to defaults.

```
sudo pmset -c gpuswitch 2

sudo pmset -b gpuswitch 0
```

Here are the flag options for pmset
```
-a - global (same behavior for charging and battery states)
-c - charging
-b - battery
```

Here are the possible options for gpuswitch
```
0 - integrated GPU only
1 - discrete GPU only
2 - autoswitch GPU
```


If you want to revert to default, run:
```
sudo pmset -a gpuswitch 2
```
which sets the global flag to autoswitch no matter which power source you're running on, replacing any individual power options set earlier.


#### Reference:
* [Battery Life on MacBook Pro 16](https://forums.macrumors.com/threads/battery-life-on-macbook-pro-16.2212813/page-4?post=28055695#post-28055695)