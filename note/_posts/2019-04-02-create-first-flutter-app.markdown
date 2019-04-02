---
layout: post
title: Create Your First Flutter App
---

```
flutter create --org com.usunyu -i swift -a kotlin --description 'First flutter app' first_flutter_app
```

`--org` sets the application prefix. On Android: `appicationId and package`. On iOS: `PRODUCT_BUNDLE_IDENTIFIER`.

`-i` sets our iOS language to swift, because Objective-C is awful by comparison.

`-a` sets our Android language to kotlin, because Java is awful by comparison.

`--description` sets our package description in our `pubspec.yaml`.

`first_flutter_app` the name of the application to create. The project is place in a folder by this same name.

#### Reference:
* [Creating your first flutter app - Part 1](https://flutter.institute/creating-your-first-flutter-app/)