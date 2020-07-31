---
title: 'Configuration Git to work with SSH'
date: 2018-04-27
permalink: /posts/2018/04/configuration-git/
tags:
  - git
  - ssh
published: true
excerpt: ""
---
#### Step 1: Installing Git

We can install Git without having to add any repositories.

apt-get install git

#### Step 2: Configuring Git
git config --global user.name "John Appleseed"

git config --global user.email "email@example.com"

#### Step 3: SSH and Git

* Check if any SSH key exists
{% highlight shell %}
ls -al ~/.ssh
{% endhighlight %}

* Generate SSH key in your .ssh directory
{% highlight shell %}
ssh-keygen -t rsa -b 4096 -C "email@example.com"
{% endhighlight %}

    NOTICE: If you use `sudo` to generate SSH key, then you cannot use `git` command without `sudo`.
Unless, you have to use `sudo` when working with `git`, try to avoid it

* Enable SSH-agen
{% highlight shell %}
eval "$(ssh-agent -s)"
{% endhighlight %}

* Add your SSH key to the SSH agent
{% highlight shell %}
ssh-add ~/.ssh/id_rsa
{% endhighlight %}

* Add your public key to your GitHub account
{% highlight shell %}
~/.ssh/id_rsa.pub
{% endhighlight %}

* Testing your connection 
{% highlight shell %}
ssh -vT git@github.com
{% endhighlight %}

