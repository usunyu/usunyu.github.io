---
layout: post
title:  "Setup ffmpeg on Xcode"
---

### Install ffmpeg, please check ([install ffmpeg](/note/2016/05/12/installing-ffmpeg-libx264-on-mac/))

### Create an Xcode Command Line Tool project

### Build Settings -> Search Paths:

##### 1. Header Search Paths: /usr/local/include

##### 2. Library Search Paths: /usr/local/Cellar/ffmpeg/3.1.1/lib

### General -> Linked Frameworks and Libraries:

##### 1. Add -> Add Other...

##### 2. Cmd + Shift + g -> /usr/local/Cellar/ffmpeg/ -> Select required dynamic libs