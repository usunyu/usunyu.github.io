---
layout: post
title:  "MySQL Command Tips"
---
#### Invoke MySQL:
{% highlight bash %}
$ mysql
$ mysql -u root -p
$ mysql --user=your-user-name --password=your-password
{% endhighlight %}

#### Create the Database:
{% highlight bash %}
mysql> CREATE DATABASE my_db;
mysql> SHOW databases;
mysql> USE my_db;
{% endhighlight %}

#### Create a Table:
{% highlight bash %}
mysql> CREATE TABLE user (id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, username CHAR(25), passward CHAR(100));
{% endhighlight %}

#### Create an entry in the table:
{% highlight bash %}
mysql> INSERT INTO user (id, username, passward) VALUES (NULL, 'name', 'secrect');
{% endhighlight %}

#### Create more entries:
{% highlight bash %}
mysql> INSERT INTO user (id, username, passward) VALUES (NULL, 'name2', 'secrect2'), (NULL, 'name3', 'secrect3'), (NULL, 'name4', 'secrect4');
{% endhighlight %}

#### Export, Import MySQL Database
{% highlight bash %}
$ mysqldump -u user -p db-name > db-name.out
$ mysql -u user -p db-name < db-name.out
{% endhighlight %}

#### Resource:
* [www.cyberciti.biz/faq/mysql-command-to-show-list-of-databases-on-server/](http://www.cyberciti.biz/faq/mysql-command-to-show-list-of-databases-on-server/)
* [www.wikihow.com/Create-a-Database-in-MySQL](http://www.wikihow.com/Create-a-Database-in-MySQL)
* [http://www.cyberciti.biz/tips/howto-copy-mysql-database-remote-server.html](http://www.cyberciti.biz/tips/howto-copy-mysql-database-remote-server.html)
