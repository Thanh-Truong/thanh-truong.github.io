---
title: 'Fix corrupted dpkg (Debian Package) ?'
date: 2022-01-17
permalink: /posts/2022/01/fix-broken-dpkg/
tags:
  - Debian
  - Google-cloud-sdk
  - dpkg
published: true
excerpt: "Unfortunately, Debian fell into a bad state, where it could not run any action."
---
# Fix corrupted dpkg
Yesterday, I ran into a probelm of `gcloud` not being able to start

```
ttruong@B5495913-NEW:/var/lib/dpkg/updates$ gcloud
ERROR: gcloud failed to load: Environment variable HTTPLIB2_CA_CERTS not a valid file
    gcloud_main = _import_gcloud_main()
    import googlecloudsdk.gcloud_main
    from googlecloudsdk.calliope import cli
    from googlecloudsdk.calliope import backend
    from googlecloudsdk.calliope import parser_extensions
    from googlecloudsdk.core.updater import update_manager
    from googlecloudsdk.core.updater import installers
    from googlecloudsdk.core import requests as core_requests
    import httplib2
    from httplib2.python3.httplib2 import *
    CA_CERTS = certs.where()
    raise RuntimeError("Environment variable HTTPLIB2_CA_CERTS not a valid file")

This usually indicates corruption in your gcloud installation or problems with your Python interpreter.

Please verify that the following is the path to a working Python 2.7 or 3.5+ executable:
    /usr/bin/python3


If it is not, please set the CLOUDSDK_PYTHON environment variable to point to a working Python 2.7 or 3.5+ executable.

If you are still experiencing problems, please reinstall the Cloud SDK using the instructions here:
    https://cloud.google.com/sdk/
```

The error indicates some sort of problem with `gcloud ` and suggests to reinstall the package.

However, this command was hanging and I had no choice but left the installation process.
```
sudo apt-get install --reinstall google-cloud-sdk
```

Due to the interupted process, the Debian package (`dpkg`) fell into a bad state. No more actions can be done

This either hangs or complains about `dpkg` being a bad state.
```
sudo apt remove google-cloud-sdk
```
This either hangs or complains about `dpkg` being a bad state.
```
sudo apt-get install google-cloud-sdk
```
Other any other `sudo apt ` ish commands as they cannot function if `dpkg` is bad.

I tried to reconfigure it but it still does not help.
```
sudo dpkg --configure -a 
```

Knowing that `dpkg` works as a database with records in plaintext, which can be modified so I did removing the bad record being `google-cloud-sdk`. 

```
sudo vi /var/lib/dpkg/status.
```
Then I reconfigured the dpkg again and crossed my fingers.
```
run sudo dpkg --configure -a.
run sudo apt-get -f install
```

I now can run `apt` related commands without errors. Hooray !

# Install google-cloud-sdk

This however does not solve my original problem, which was installing `google-cloud-sdk`. I got the following errors towards the end of the installation. By the way, the installation instruciton can be found [here](https://cloud.google.com/sdk/docs/install#deb)

```
Setting up google-cloud-sdk (368.0.0-0) ...
Scanning processes...
Scanning candidates...
Scanning processor microcode...
Scanning linux images...

Failed to retrieve available kernel versions.

Failed to check for processor microcode upgrades.

No services need to be restarted.

No containers need to be restarted.

User sessions running outdated binaries:
 root @ /dev/tty1: init[7], sh[2454]
 ttruong @ /dev/tty1: bash[8]
 ```
 