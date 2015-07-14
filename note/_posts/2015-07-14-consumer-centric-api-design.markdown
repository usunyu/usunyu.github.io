---
layout: post
title:  "Consumer-Centric API Design"
---
#### Examples of Abstraction:
##### 1. Good Abstraction
{% highlight bash %}
Creating a Notification
POST /notifications
{
  "user_id": "12",
  "message": "Hello World"
}
{
  "id": "1000",
  "user_id": "12",
  "message": "Hello World",
  "medium": "email",
  "created": "2013-01-06T21:02:00Z"
}
{% endhighlight %}