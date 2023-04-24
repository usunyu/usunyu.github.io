---
layout: post
title: Optimize macOS Dock Autohide Animation & Delay
---

```
defaults write com.apple.dock autohide-delay -float 0; defaults write com.apple.dock autohide-time-modifier -float 0.5; killall Dock
```

#### Resource:

- [Autohide animation time](https://macos-defaults.com/dock/autohide-time-modifier.html)
- [Autohide delay](https://macos-defaults.com/dock/autohide-delay.html)
