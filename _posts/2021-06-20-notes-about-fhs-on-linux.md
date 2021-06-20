---
title: 'Filesystem Hierarchy Standard'
date: 2021-06-20
permalink: /posts/2021/06/notes-fhs-on-linux/
tags:
  - FHS
  - Ubuntu
  - Linux
  - OS
published: true
excerpt: "Sometimes it is difficult to grasp directory structure in Linux/Ubuntu."
---
Sometimes it is difficult to grasp directory structure in Linux/Ubuntu without referring to [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard), FHS, which defines directory structure and directory contents in Linux distributions. 

In the FHS, all files and directories appear under the root directory `/`, even if they are physically on different disks or devices.

# Binaries and libraries
## System scope
* `/bin`
For binaries usable before the `/usr` partition is mounted. This is used for trivial binaries used in the very early boot stage or ones that you need to have available in booting single-user mode.For example: cat, ls, cp.

* `/sbin`
System binaries (e.g., fsck, init, route), available for `root`

* `/lib`
Libraries for the binaries in `/bin` and `/sbin`.

## User scope
* `/usr/bin`
Command binaries (not needed in single-user mode); for all users.

* `/usr/lib`
Libraries for the binaries in `/usr/bin` and `/usr/sbin`.

* `/usr/local`
It has subdirectories (e.g., bin, lib, share) for local data specifically for to the host.

* `/usr/sbin`
Non-essential system binaries (e.g., daemons for various network services).

## Variable files
* `/var`
Files such as logs, spool files, and temporary e-mail files, whose content are continually changed during operation of the system.

* `/var/cache`
Application cache data locally generated as during I/O or calculation.

* `/var/lib`
State information. Persistent data modified by programs as they run (e.g., databases, packaging system metadata, etc.).

* `/var/lock`
Lock files. Files keeping track of resources currently in use.

* `/var/log`
Log files.

* `/var/opt`
Variable data from add-on packages that are stored in /opt.

* `/var/tmp`
Temporary files to be preserved between reboots.

# Others

* `/`
Primary hierarchy root and root directory of the entire file system hierarchy.

* `/boot`
Boot loader files (e.g., kernels, initrd).
* `/dev`
Device files (e.g., /dev/null, /dev/disk0, /dev/sda1, /dev/tty, /dev/random).
* `/etc`
Host-specific system-wide configuration files.
* `/etc/opt`
Configuration files for add-on packages that are stored in /opt.
* `/home`
Users' home directories, containing saved files, personal settings, etc.

* `/media`
Mount points for removable media such as CD-ROMs (appeared in FHS-2.3 in 2004).

* `/opt`
Add-on application software packages.

* `/root`
Home directory for the root user.

* `/sys`
Contains information about devices, drivers, and some kernel features.
* `/tmp`
Directory for temporary files (see also /var/tmp), and they are not preserved between system reboots and can be size-restricted.

* `/usr`
Secondary hierarchy for read-only user data; contains the majority of (multi-)user utilities and applications. Should be shareable and read-only.

* `/usr/include`
Standard include files.


# Where should I put my script ?
The answer is none of the above. I suggest to use `/usr/local/bin` to make available for system-wide unless I use `~/bin` for user scoped script. The script and its data are located under personal home directory.

Under `/usr/local` there are potentially more subdirectories to accommodate libraries or data, e.g., `/usr/local/bin`, `/usr/local/lib`, `/usr/local/share`.