---
layout: post
title: How to reset Django migrations
---

#### Remove the all migrations files within your project:
Go through each of your projects apps migration folder and remove everything inside, except the __init__.py file.

Or if you are using a unix-like OS you can run the following script (inside your project dir):

{% highlight bash %}
$ find . -path "*/migrations/*.py" -not -name "__init__.py" -delete
$ find . -path "*/migrations/*.pyc"  -delete
{% endhighlight %}

#### Drop the current database, or delete the db.sqlite3 if it is your case.

#### Create the initial migrations and generate the database schema:

{% highlight bash %}
$ python manage.py makemigrations
$ python manage.py migrate
{% endhighlight %}

And you are good to go.

#### Reference:
* [How to Reset Migrations](https://simpleisbetterthancomplex.com/tutorial/2016/07/26/how-to-reset-migrations.html)