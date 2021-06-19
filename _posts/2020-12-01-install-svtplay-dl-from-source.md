---
title: 'Install svtplay-dl from source'
date: 2020-12-01
permalink: /posts/2020/12/install-svtplay-from-source/
tags:
  - svtplay-dl
published: true
excerpt: "Svtplay-dl"
---
This note is taken from my effort to install [`svtplay-dl`](https://svtplay-dl.se), which is an open source command-line program that enables downloading videos from various sites including [SVT Play](svtplay.se), [TV4 Play](tv4play.se).

Detailed instructions to install this utility can be on on the official page [`svtplay-dl`](https://svtplay-dl.se). However, this notes tries to install and build the utility directly from source, which allows customizing the utility and definitely this is the best way to keep up-to-date with bug fixes and patches.

First, we begin with cloning the source code to a folder
```
git clone https://github.com/spaam/svtplay-dl.git
````

Remember to run `git pull` to sync with the `master` branch if you have cloned `svtplay-dl` before.

Now, it is time to make sure that `python3` is already be installed. Otherwise, just install it preferably with  with `brew`
````
brew install python3
````

Then we install a bunch of required packages
````
pip3 install requests
```
RTMPDump 2.4 should already be installed with brew, if not:
````
brew install rtmpdump
```
then
```
pip3 install cryptography

pip3 install pyyaml

pip3 install pysocks

brew install ffmpeg
````

Within the folder, we build the source code and then install it

```
make
sudo make install
````

Finally, we need to test the installation
```
svtplay-dl --version
````

and you should see some new version which is accordingly to the newest source code on Github, something looks like

svtplay-dl 2.1-53-gd33186e

It is always good to remove other versions of `svtplay-dl` that you had installed via `brew``
```
brew uninstall svtplay-dl
```