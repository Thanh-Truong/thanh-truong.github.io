---
title: 'Install svtplay-dl from source'
date: 2020-12-01
permalink: /posts/2020/12/install-svtplay-from-source/
tags:
  - svtplay-dl
published: true
excerpt: "Svtplay-dl"
---
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
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