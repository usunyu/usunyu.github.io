---
layout: post
title: Installing the IBAPI Package
---

1. Download "API Latest" from [http://interactivebrokers.github.io/](http://interactivebrokers.github.io/)

2. Unzip or install (if its a .msi file) the download.

3. Go to `tws-api/source/pythonclient/`

4. Build a wheel with:

```
python setup.py bdist_wheel
```

5. Install the wheel with:

```
pip install --upgrade dist/ibapi-9.81.1-py3-none-any.whl
```

#### Reference:

- [Installing the ibapi package](https://stackoverflow.com/questions/57618117/installing-the-ibapi-package)
