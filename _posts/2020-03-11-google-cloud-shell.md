---
title: 'Remotely running Visual Studio Code on Google Cloud Shell'
date: 2020-03-11
permalink: /posts/2020/03/google-cloud-shell/
tags:
  - gcp
  - visual-studio-code
published: true
excerpt: ""
---
[Google Cloud Shell](https://cloud.google.com/shell): 
It is a hidden germ that mostly goes under the rader. 
> Cloud Shell provides you with command-line access to your cloud resources directly from your browser. You can easily manage your projects and resources without having to install the Google Cloud SDK or other tools on your system. 

Some keys to take away
-  Google Shell is provisoned by e2-small Google Compute Engine
-  Limited storage 5G
-  But Google Cloud Shell is free for users with a Google Cloud Platform account.
-  It is secured
-  It has most of command line tools ( gcloud, mvn, git, ..) and text editors (nano, vim, emacs, etc) which are all that you need
-  It supports Java, Python, PhP, .NET, Go

# Starting Google Cloud Shell

- Option 1 - via Google Cloud Console
    - Open a webbrowser and visit http://console.cloud.google.com/ and switch to your project of choice
    - Click on the icon `\>` on top
    
- Option 2 - via Google Cloud Shell command line
    - Currently, this works only with `gcloud alpha`, so `gcloud component install alpha`
    - Then `gcloud cloud shell ssh --dry-run`, which shows which dry-runs the login
    - Then `gcloud cloud shell ssh` to start a Google Cloud Shell and login.
 
 # Installing Visual Code on Google Cloud Shell
 
 To do this, I am using > code-server is VS Code running on a remote server, accessible through the browser.
 `code-server` is available at https://github.com/cdr/code-server

 The first script named as `download_vs.sh` is to download a latest code-server binary 

  {% highlight shell %}
  #!/bin/bash

  export VERSION=`curl -s https://api.github.com/repos/cdr/code-server/releases/latest | grep -oP '"name": "\K(.*)(?=")' | head -n 1`
  #wget https://github.com/cdr/code-server/releases/download//code-server-linux-x64.tar.gz
  wget https://github.com/cdr/code-server/releases/download/2.1698/code-server2.1698-vsc1.41.1-linux-x86_64.tar.gz
  tar -xvzf code-server2.1698-vsc1.41.1-linux-x86_64.tar.gz
  cd code-server2.1698-vsc1.41.1-linux-x86_64
  {% endhighlight %}

  Then the second script named as `run_vs.sh` is to start Visual Code
  {% highlight shell %}
  #!/bin/bash
  cd code-server2.1698-vsc1.41.1-linux-x86_64
  sudo ./code-server --no-auth --auth 'none' --port 8080
  {% endhighlight %}

# Accessing Visual Code from a browser
- Option 1 - via Google Cloud Console
  There is an icon `Web preview` to launch another web tab to interact with the Visual Studio.
  If there is an error saying that the address could not be found, please remove the parameter `auther` 
  
- Option 2 - via Google Cloud Shell command line
  {% highlight shell %}
  #!/bin/bash
  gcloud alpha cloud-shell ssh --ssh-flag="-L 8080:localhost:8080"
  {% endhighlight %}
  
  This opens a connection to the Google Cloud Shell server, and forwards any connection to port 8080 on the local machine to port 8080 on the Google Cloud Shell server.

 Enjoy !!!

![](/images/visual_studio_code_server.png)
