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

Directory	Description
# Binaries and libraries
## System scope
* `/bin`
Essential command binaries that need to be available in single-user mode, including to bring up the system or repair it,[4] for all users (e.g., cat, ls, cp).
* `/sbin`
Essential system binaries (e.g., fsck, init, route).
* `/lib`
Libraries essential for the binaries in /bin and /sbin.
## User scope
* `/usr/lib`
Libraries for the binaries in /usr/bin and /usr/sbin.
* `/usr/local`
Tertiary hierarchy for local data, specific to this host. Typically has further subdirectories (e.g., bin, lib, share).[NB 1]
* `/usr/sbin`
Non-essential system binaries (e.g., daemons for various network services).

# Variable files
* `/var`
Variable files: files whose content is expected to continually change during normal operation of the system, such as logs, spool files, and temporary e-mail files.
* `/var/cache`
Application cache data. Such data are locally generated as a result of time-consuming I/O or calculation. The application must be able to regenerate or restore the data. The cached files can be deleted without loss of data.
* `/var/lib`
State information. Persistent data modified by programs as they run (e.g., databases, packaging system metadata, etc.).
* `/var/lock`
Lock files. Files keeping track of resources currently in use.
* `/var/log`
Log files. Various logs.
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
* `/mnt`
Temporarily mounted filesystems.
* `/opt`
Add-on application software packages.[8]
* `/root`
Home directory for the root user.

* `/sys`
Contains information about devices, drivers, and some kernel features.
* `/tmp`
Directory for temporary files (see also /var/tmp). Often not preserved between system reboots and may be severely size-restricted.
* `/usr`
Secondary hierarchy for read-only user data; contains the majority of (multi-)user utilities and applications. Should be shareable and read-only.[10][11]
* `/usr/bin`
Non-essential command binaries (not needed in single-user mode); for all users.
* `/usr/include`
Standard include files.

