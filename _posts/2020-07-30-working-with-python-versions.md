---
title: 'How to switch between Python versions'
date: 2020-07-03
permalink: /posts/2020/07/working-with-python-versions/
tags:
  - python
published: true
excerpt: ""
---
Even though it was [sunsetting](https://www.python.org/doc/sunset-python-2/) of Python 2 by the begin of 2020 but there are cases that we still need to work on Python 2. The world is not perfect and not everyone was ready to move to Python 3. This note provides some instructions for you to handle different versions of Python locally.

Make sure that there is no alias for python

{% highlight shell %}
$ type python
{% endhighlight %}

In case, it is, just unalias it

{% highlight shell %}
$ alias python
{% endhighlight %}

List all installed python(s)
{% highlight shell %}
$ ls -larth `which python`*
-rwxr-xr-x 1 root root   846 Mar 10 03:03 /usr/local/bin/python
-rwxr-xr-x 2 root staff  15M Mar 10 03:34 /usr/local/bin/python3.7m
-rwxr-xr-x 2 root staff  15M Mar 10 03:34 /usr/local/bin/python3.7
-rwxr-xr-x 1 root staff 3.1K Mar 10 03:35 /usr/local/bin/python3.7m-config
lrwxrwxrwx 1 root staff   16 Mar 10 03:35 /usr/local/bin/python3-config -> python3.7-config
lrwxrwxrwx 1 root staff   17 Mar 10 03:35 /usr/local/bin/python3.7-config -> python3.7m-config
lrwxrwxrwx 1 root staff    9 Mar 10 03:35 /usr/local/bin/python3 -> python3.7
{% endhighlight %}

Then we need to update alternatives for python with the syntax

{% highlight shell %}
sudo update-alternatives --install <link> python <path> <priority>
{% endhighlight %}
So, it looks like

{% highlight shell %}
$ sudo update-alternatives --install /usr/bin/python python /usr/local/bin/python3 1
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 2
{% endhighlight %}
Lastly, we need to config which Python we want to use as default

{% highlight shell %}

udo update-alternatives --config python
There are 2 choices for the alternative python (providing /usr/bin/python).

  Selection    Path                    Priority   Status
------------------------------------------------------------
* 0            /usr/bin/python2.7       2         auto mode
  1            /usr/bin/python2.7       2         manual mode
  2            /usr/local/bin/python3   1         manual mode

Press <enter> to keep the current choice[*], or type selection number: 2
update-alternatives: using /usr/local/bin/python3 to provide /usr/bin/python (python) in manual mode
{% endhighlight %}

Test

{% highlight shell %}
python --version
Python 3.7.3
{% endhighlight %}
