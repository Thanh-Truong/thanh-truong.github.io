---
title: 'Script to get free port Unix/Linux/OSX'
date: 2014-03-14
permalink: /posts/2014/03/get-free-port-unix-osx-windows/
tags:
  - C
  - network
published: true
excerpt: ""
---
The following script is originally from my own project. It returns a free port starting from configurable port number.

{% highlight shell %}
#!/bin/bash
 
# Default nameserver port
(( port=35021 ))
 
# Silently (&gt;&gt; /dev/null) see if the port is free,
# otherwise increase counter to find a free port
if [ &quot;$(uname)&quot; == &quot;Darwin&quot; ]; then
# Special test for Mac OS X platform
 while lsof -n -i4TCP:$port | grep LISTEN &gt;&gt; /dev/null
 do
 (( port += 1 ))
 done
else
 while netstat -antu | grep $port &gt;&gt; /dev/null
 do
 (( port += 1 ))
 done
fi
 
echo $port
{% endhighlight %}

If you want to close an open port fiercely. Here you go

{% highlight shell %}
sudo netstat -ap | grep :<port_number> 
{% endhighlight %}

It returns a corresponding process holding the port_number. Then you kill it by 

{% highlight shell %}
kill pid 
or kill -9 pid 
{% endhighlight %}
