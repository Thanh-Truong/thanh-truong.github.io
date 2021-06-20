---
title: 'Enable Remote Access to MySQL server running on Unix machine'
date: 2012-09-27
permalink: /posts/2012/09/access-to-mysql-on-ubuntu/
tags:
  - mysql
  - database
  - ubuntu
published: true
excerpt: ""
---
This note is written as I was working on machine X running Scientific Linux 6.0 or above

a ) Install MySQL on server X

b) Modify /etc/my.cnf to include the following configuration under `mysqld` group

{% highlight shell %}
bind-address=130.238.xxxx
{% endhighlight %}

in which 130.238.9.xxxx is IP address of X

c) Start MySQL server
{% highlight shell %}
sudo /etc/init.d/mysqld
{% endhighlight %}

Notes:

- Stop by `sudo /etc/init.d/mysqld`

- Status by `sudo mysqladmin status`

- Don't want to change my.cnf you can `sudo /etc/init.d mysqld bind-address=130.238.xxxx`

d) Open another SSH connect to X and test
{% highlight shell %}
mysql -u root
{% endhighlight %}

Make sure that you can login

e) When you are in MySQL, run the following scripts to create a new database, a new user, and to grant the usage, privileges to the just created user

{% highlight shell %}
CREATE USER 'regress'@'localhost' IDENTIFIED BY 'regress';

GRANT USAGE ON * . * TO 'regress'@'localhost' IDENTIFIED BY 'regress'
 WITH MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0
 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0 ;

GRANT USAGE ON * . * TO 'regress'@'130.238.11.215' IDENTIFIED BY 'regress'
 WITH MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0
 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0 ;

CREATE DATABASE IF NOT EXISTS `regress` ;

GRANT ALL PRIVILEGES ON `regress` . * TO 'regress'@'localhost';

GRANT ALL PRIVILEGES ON `regress` . * TO 'regress'@'130.238.11.yyyy';
{% endhighlight %}

f) Make sure that the scripts you ran affected the database

With root login, you can run and observe that fact

{% highlight shell %}
show grants for regress@'130.238.11.yyyy';

 +-------------------------------------+
 | Grants for regress@130.238.11.yyyy |
 +------------------------------------+
 | GRANT USAGE ON *.* TO 'regress'@'130.238.11.yyyy' IDENTIFIED BY PASSWORD '*A90B6F0C025D525A27B5A18F8FD2474AE499BAD4' |
 | GRANT ALL PRIVILEGES ON `regress`.* TO 'regress'@'130.238.11.yyyy' |
 +------------------------------------+
{% endhighlight %}

g) Now it is time to open a new rule in your iptables ( Firewall configuration)

Allow only connection from your local machine.
{% highlight shell %}
iptables -A INPUT -i eth0 -s 130.238.11.215  -p tcp --destination-port 3306 -j ACCEPT
{% endhighlight %}

h) Now open a terminal and test the result

{% highlight shell %}
C:\Users\thanh&gt;mysql -h X.it.uu.se -u regress -p regress --port=3306
 Enter password: *******
 Welcome to the MySQL monitor. Commands end with ; or \g.
 Your MySQL connection id is 9
 Server version: 5.1.61 Source distribution

Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

mysql&gt; exit;
 Bye
{% endhighlight %}

That's all !

I can run two instances of MySQL server

a) mysql -u regress -p regress

b) mysql -u regress -P 3310 --protocol=tcp -p regress

in which 3310 is a port number on which another MySQL is listening for incoming requests