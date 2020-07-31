---
title: 'Useful commands with Git'
date: 2014-03-11
permalink: /posts/2014/03/github-useful-commands/
tags:
  - git
  - commands
published: true
excerpt: ""
---
A list of Git commands that are practically useful even after years of experience

### Repository Aladdin
* Create new respository by GitHub webinterface
* git init
* git remote add origin git@github.com:Thanh-Truong/Aladdin.git
* git push -u origin master

### Work routine:

* only read [Participating](https://github.com/notifications/participating) that someone mentions you.
* when you have more time or get bored [Notifications](https://github.com/notifications) 
* if you even have time, check new [issues](https://github.com/issues) that are interesting to follow

### Shortcuts

* [Assigned](https://github.com/issues/assigned).
* [Pulls](https://github.com/issues/pulls)

### Searching issues and pull requests
https://help.github.com/articles/searching-issues-and-pull-requests/

### More tips 

* [Refine Github](https://github.com/sindresorhus/refined-github) experience 
* If you want to know what your colleagues are doing on Github ? Check [Devspace](https://devspace.io/)

### Commands

* Reset changes
{% highlight shell %}
git reset --hard
{% endhighlight %}

* New branch locally
{% highlight shell %}
git branch xx
git checkout typexx
{% endhighlight %}

* Fetch new branch from remote
{% highlight shell %}
git fetch origin xx
git checkout xx
{% endhighlight %}

* List branch locally
{% highlight shell %}
git branch --list
{% endhighlight %}

* List branch remote
{% highlight shell %}
git branch -r
{% endhighlight %}

* List branch both local and remote
{% highlight shell %}
git branch -a
{% endhighlight %}

* Delete xx on local
{% highlight shell %}
git branch xx -D
{% endhighlight %}

* Delete xx on remote. Here origin is my remote
{% highlight shell %}
git push origin :xx
{% endhighlight %}

* Pull xx from remote
{% highlight shell %}
git pull origin xx
{% endhighlight %}

* Merge xx from remote
{% highlight shell %}
git merge origin xx
git merge origin xx --no-ff
{% endhighlight %}

* Squash all commit from a feature branch, which diverges from this branch
{% highlight shell %}
git merge origin xx --squash
{% endhighlight %}

* Rename old branch
{% highlight shell %}
#Rename branch locally 
git branch -m old_branch new_branch         
#Delete the old branch 
git push origin :old_branch                     
#Push the new branch, set local branch to track the new remote
git push --set-upstream origin new_branch   
{% endhighlight %}

* showing parents of a merge commit
{% highlight shell %}
git show --pretty=raw <commit hash> 
{% endhighlight %}

