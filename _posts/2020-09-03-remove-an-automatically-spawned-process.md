---
title: 'Remove an automatically spawned process'
date: 2020-09-03
permalink: /posts/2020/09/remove-an-automatically-spawned-process/
tags:
  - osx
  - bash
published: true
excerpt: ""
---
This note was inspired by a [discussion](https://apple.stackexchange.com/questions/25287/how-do-you-prevent-a-process-from-automatically-restarting-specifically-sopho) on StackOverflow.

Recently I got a new computer from work and it was pre-installed with some softwares from IT support service. Some of them are useful to aid IT support to remotely install packages on my computer while some aim at collecting logs or controlling network usage. I have no problem running them when needed but they are auto launched at startup, which are unwanted background processes. And that is what I don't like about them. Killing them all does not work as they are automatically spawned by `launchctl`.

So here is how I contain them in more control manner. To give a concrete example of such preinstalled software, I use [`MacApp Kiosk`](https://www.filewave.com/management/self-service), which has executable process called as `FileWave.app`. This knowledge can be obtained by using `Activity Monitor` tool shipped in any OSX.

* List all related processes to `FileWave`.
{% highlight shell %}
$ ps aux | grep 'FileWave' | grep -v grep
root              7080   0,1  0,0  5763108   5072   ??  SNs  10:03pm   0:01.70 /usr/local/sbin/FileWave.app/Contents/MacOS/fwcld
thtru             7002   0,0  0,0  6110104  15252   ??  SN   10:03pm   0:02.79 /usr/local/sbin/FileWave.app/Contents/Resources/fwGUI.app/Contents/MacOS/fwGUI
{% endhighlight %}


* Now, we need to find which parent process that keeps on spawning the `FileWave` app (front-end application as `fwGUI`) whose id is `7002`.

{% highlight shell %}
$ ps aux -o ppid | grep 7002 | grep -v grep
thtru             7002   0,0  0,0  6110104  15244   ??  SN   10:03pm   0:02.83 /usr/local/sbin/     1
{% endhighlight %}

As expected we could not find who is the parent process but we got a hint that was indeed a system process.

* This led to the other possibility that is the process is being kept alive by the launch daemon `launchd`. We need to examine all possibilities that our targeted process was configured to be launched somewhere among these locations.

{% highlight shell %}
$ ls ~/Library/LaunchAgents | grep 'fwGUI'
$ ls /Library/LaunchAgents | grep 'fwGUI'
$ ls /Library/LaunchDaemons | grep 'fwGUI'
$ ls /System/Library/LaunchAgents | grep 'fwGUI'
{% endhighlight %}

 The result showed that there is an entry `*.plist` as XML file that configures how the `fwGUI` should be launched. 
{% highlight shell %}
$ ls /Library/LaunchAgents | grep 'fwGUI'
com.filewave.fwGUI.plist
{% endhighlight %}

 The file contains several parameter and we can see it has `KeepAlive` parameter, which is exactly we don't want to have. Remove this parameter will stop `FileWave` app to be spawned after we kill it manually.

 To completely suppress `FileWave` from running when we login, we can go ahead and remove the plist file.