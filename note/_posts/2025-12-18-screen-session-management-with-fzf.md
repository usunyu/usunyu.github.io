---
layout: post
title: Screen Session Management with fzf
---

**Prerequisites**
```bash
sudo apt update
sudo apt install -y screen fzf
```

**Add screen + fzf Shortcut**

Edit ~/.bashrc::
```bash
vim ~/.bashrc
```

Append the following function:
```bash
# screen + fzf attach
s() {
  local session
  session=$(screen -ls | awk '/Detached/ {print $1}' | fzf)
  if [ -n "$session" ]; then
    screen -r "$session"
  fi
}
```

Apply changes:
```bash
source ~/.bashrc
```

**Usage**
```bash
s
```

* Shows all **Detached** screen sessions in an interactive list
* Type to filter, **↑ / ↓** to select
* Press **Enter** to attach