---
layout: post
title: Create Rails Project
---

#### [Getting Started with Rails 4.x on Heroku](https://devcenter.heroku.com/articles/getting-started-with-rails4):
{% highlight bash %}
$ rails new test_project -d mysql
$ bundle install
{% endhighlight %}
{% highlight bash %}
$ rails server
$ rails s
{% endhighlight %}
{% highlight bash %}
$ heroku login
// add to gem 'rails_12factor', group: :production Gemfile
$ bundle install
// add to  ruby “2.0.0” Gemfile
// in App folder
$ git init
$ git add .
$ git commit -m "init"
$ heroku create
$ git config -e
{% endhighlight %}

#### [Managing Your SSH Keys](https://devcenter.heroku.com/articles/keys):
{% highlight bash %}
$ ssh-keygen -t rsa
$ heroku keys:add
{% endhighlight %}
{% highlight bash %}
$ heroku run rake db:migrate
{% endhighlight %}
{% highlight bash %}
$ heroku ps:scale web=1
$ heroku ps
$ heroku open
$ heroku logs
$ heroku logs —tail
{% endhighlight %}
{% highlight bash %}
$ heroku addons:add cleared
{% endhighlight %}

#### [ClearDB MySQL Database](https://devcenter.heroku.com/articles/cleardb):
{% highlight bash %}
$ heroku config | grep CLEARDB_DATABASE_URL
$ heroku config:set DATABASE_URL='CLEARDB_DATABASE_URL'
{% endhighlight %}
