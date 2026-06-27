---
layout: post
title: Limit MongoDB Memory Usage
---

**1. Update config**
```bash
sudo vim /etc/mongod.conf
```

Add / modify:
```bash
storage:
  dbPath: /var/lib/mongodb
  wiredTiger:
    engineConfig:
      cacheSizeGB: 0.25
```

**2. Restart MongoDB**
```bash
sudo systemctl restart mongod
```

**3. Verify**
```bash
free -h
```

**Notes**
* Recommended: ~10%–25% of total RAM
* For 4GB machine: 0.5