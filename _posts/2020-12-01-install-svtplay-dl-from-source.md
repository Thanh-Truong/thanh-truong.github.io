---
title: 'Install svtplay-dl from source'
date: 2020-12-01
permalink: /posts/2020/12/install-svtplay-from-source/
tags:
  - svtplay-dl
published: false
excerpt: ""
---
Everything should be installed via [Homebrew](https://brew.sh/index_sv) for the sake of simplicity and ease of uninstallation

## Install Homebrew

{% highlight shell %}
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
{% endhighlight %}

It is quite common to be confused between `brew` and `brew cask`. In the nut shell, `brew` installs command-line softwares while `brew cask`installs graphical applications and it deals with licencees.

## Install git with ssh

I find my old post about [Git and SSH](/posts/2018/04/configuration-git/) useful indeed

## Install iTerm2
{% highlight shell %}
brew cask install iterm2
{% endhighlight %}

Before heading to the next headline, there is some good words here 
https://sourabhbajaj.com/mac-setup/iTerm/zsh.html

## Install Oh My Zsh
This [Oh My Zsh](https://ohmyz.sh) is a framework built to manage [Zsh](https://www.zsh.org/) configuration. In its own world, Zsh is a nicely designed interactive shell that I love to use when I develop. There are some better alternatives but I haven't got time to try them out.

* First step is off course, install Zsh
{% highlight shell %}
brew install zsh
{% endhighlight %}

More instructions about how to install Zsh on different OS can be found [here] 
(https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)

* Invoke `zsh` for the first time will take you to the configuration menu where a shitload of options to be configured. The result should be a `.zshsrc` file
more ~/.zshrc
{% highlight shell %}
brew install zsh

# Lines configured by zsh-newuser-install
HISTFILE=~/.histfile
HISTSIZE=1000
SAVEHIST=1000
setopt nomatch
unsetopt autocd beep
bindkey -e
# End of lines configured by zsh-newuser-install
# The following lines were added by compinstall
zstyle :compinstall filename '/Users/thtru/.zshrc'

autoload -Uz compinit
compinit
# End of lines added by compinstall
{% endhighlight %}

* Themes
Simply choose a theme you like for example

```
# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
ZSH_THEME="pygmalion"
```

The best instruction so far for on "Powerlevel9k", a very good theme
https://gist.github.com/kevin-smets/8568070

## Install Emacs

Emacs is always my favourite editor especially for a quick change.

```
brew install emacs
```
and it is invoked with simply `emacs` command

To be continued


The source is the same for all OS. This is what i did if i remember it correctly.

Open a terminal window

install brew

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
clone to a folder
git clone https://github.com/spaam/svtplay-dl.git

cd to the folder svtplay-dl was cloned to
cd svtplay-dl
(if you already have the folder svtplay-dl you need to run git pull to sync with master and you should continue with step 12)

python3 should already be installed with brew, but try to run python to see if it is added to the path or install it with brew install python3

pip3 install requests

RTMPDump 2.4 should already be installed with brew, if not:
brew install rtmpdump

pip3 install cryptography

pip3 install pyyaml

pip3 install pysocks

brew install ffmpeg

make

sudo make install

to test the installation
svtplay-dl --version
and you should see
svtplay-dl 2.1-53-gd33186e
if not, try to uninstall the older version with
brew uninstall svtplay-dl