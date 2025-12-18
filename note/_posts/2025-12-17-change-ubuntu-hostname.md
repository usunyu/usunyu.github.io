---
layout: post
title: Change Hostname on Ubuntu
---

**Set the hostname**
```bash
sudo hostnamectl set-hostname frankfurt-1
```

**Update /etc/hosts (IMPORTANT)**

Edit the file:
```bash
sudo vim /etc/hosts
```

Ensure it contains:
```bash
127.0.0.1 localhost
127.0.1.1 frankfurt-1

::1 ip6-localhost ip6-loopback
```

**Reload the shell**
```bash
exec bash
```

**Verify**
```bash
hostname
hostnamectl
```

**Notes**
* No reboot required
