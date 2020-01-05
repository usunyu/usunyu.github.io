---
layout: post
title: Unity Location of downloaded Asset files
---

You rarely need to access the files downloaded from the Asset Store directly. However, if you do need to, you can find them in the following paths:

macOS:
```
~/Library/Unity/Asset Store
```

Windows:
```
C:\Users\accountName\AppData\Roaming\Unity\Asset Storesudo umount /vol
```

These folders contain subfolders that correspond to particular Asset Store vendors. The actual Asset files are contained inside the subfolders using a structure defined by the Asset package publisher.

#### Reference:
* [Using the Asset Store](https://docs.unity3d.com/Manual/AssetStore.html)