# Linux Filesystem Hierarchy Standard (FHS)

# Introduction

## Why there are a standard for linux filesystem

The standard enables both users and software to predict the location of installed files and directories. This is done by specifying the layout of the file system and the content of the files and directories.

It also enables both os and software developers write applications that are FHS compliant. So it is easier for users to use and manage the system in expected ways.

# The Root Filesystem

## Purpose

The root filesystem is the top-level directory of the filesystem. It must contain the files and directories needed to boot, repair, recover the system and start the system daemons.

> tip: the designers aim to keep the root filesystem as small as possible to be able to boot the system from a variety of devices (including small devices) and minimize the risk of data corruption.

Application must not create files or directories in the root filesystem. Other locations in the filesystem are more the enough for that. (remember that we aim to keep the root filesystem as small as possible)

## Required directories

The following directories are required in the root filesystem:

| Directory | Description                                       |
| --------- | ------------------------------------------------- |
| `/bin`    | Essential command binaries                        |
| `/boot`   | Static files of the boot loader                   |
| `/dev`    | Device files                                      |
| `/etc`    | Host-specific system-wide configuration files     |
| `/lib`    | Essential shared libraries and kernel modules     |
| `/media`  | Mount point for removable media                   |
| `/mnt`    | Mount point for mounting a filesystem temporarily |
| `/opt`    | Add-on application software packages              |
| `/sbin`   | Essential system binaries                         |
| `/srv`    | Data for services provided by this system         |
| `/tmp`    | Temporary files                                   |
| `/usr`    | Secondary hierarchy                               |
| `/var`    | Variable data                                     |

## Optional directories

The following directories are optional in the root filesystem:

| Directory    | Description                                 |
| ------------ | ------------------------------------------- |
| `/home`      | User home directories                       |
| `/root`      | Home directory for the root user            |
| `/lib<qual>` | Alternate format essential shared libraries |

> The key difference between required and optional directories is:
>
> - Required directories must be present on any system, they contains the core system files.
> - Optional directories are not required for the system to function, but they are related to user management, libraries and administrative purposes.

You can see the root directory and navigate it in your terminal, by default, you terminal is in the `/home/<username>` directory (denoted by a `~` prefix):

```bash
taha@taha:~$
```

to navigate to the root directory, you can use the `cd` command:

```bash
taha@taha:~$ cd /
taha@taha:/$ ls
bin  boot  cdrom  dev  etc  home  lib  lib32  lib64  libx32  lost+found  media  mnt  opt  proc  root  run  sbin  snap  srv  swapfile  sys  tmp  usr  var
```

to go back to the home directory, you can:

- use the `cd ~` command
- use the `cd` command without any arguments
- use the `cd /home/<username>` command

## `/bin`

The `/bin` directory contains essential command binaries used by both the system administrators and users, it also contains commands that are used indirectly by scripts.

There must no be subdirectories in `/bin`.

The following commands are found in the `/bin` directory:

| Command | Description |
| ------- | ----------- |
| `cat`   | Concatenate and display the content of files |
| `chgrp` | Change group ownership |
| `cp`    | Copy files and directories |
| `ls`    | List directory contents |
| `mkdir` | Create directories |
| `mv`    | Move files and directories |
| `rm`    | Remove files or directories |
| `rmdir` | Remove empty directories |
| `date`  | Display or set the date and time |
| `true`  | Do nothing, successfully |
| `false` | Do nothing, unsuccessfully |
| `hostname` | Show or set the system's host name |
| `kill`  | Send a signal to a process |
| `ps`    | Report a snapshot of the current processes |
| `pwd`   | Print the name of the current working directory |

and more

## `/boot`

The `/boot` directory contains the files needed to boot the system except the configuration files that are not needed at boot time.

The operating system kernel must be located in either `/` or `/boot`

## `/dev`

The `/dev` directory contains device files, these files are used to access hardware devices and other system resources.

## `/etc`

The `/etc` directory contains host-specific system-wide configuration files. It is a central location for all system-wide configuration files.

A configuration file is a local file used to control the operation of a program, it must be static and should not be an executable

## `/home`

The `/home` directory contains the home directories for users. It is a place for users to store their personal files and directories.

## `/lib`

The `/lib` directory contains essential shared libraries and kernel modules.

## `/media`

The `/media` directory is used as a mount point for removable media such as CD-ROMs, USB drives, etc.

## `/mnt`

The `/mnt` directory is used as a temporary mount point for mounting a filesystem temporarily.

## `/opt`

The `/opt` directory is used for add-on application software packages. Software packages that are optional and not part of the base system are installed in this directory.

A package installed in `/opt` must locate its static files in a separate `/opt/<package>` or `/opt/<provider>` directory.

## `/root`

The `/root` directory is the home directory for the root user.

## `/sbin`

The `/sbin` directory contains essential system binaries. It contains commands that are used by the system administrator for system maintenance.

`/sbin` contains binaries essential for booting, restoring, recovering, and/or repairing the system in addition to the binaries in `/bin`

the following commands are found in the `/sbin` directory:

| Command | Description |
| ------- | ----------- |
| `shutdown` | Shutdown the system |
| `reboot`   | Reboot the system |
| `halt`     | Stop the system |
| `fasthalt` | Quickly stop the system |
| `fastboot` | Quickly boot the system |
| `ifconfig` | Configure network interfaces |
| `route`    | Show or manipulate the IP routing table |

## `/srv`

The `/srv` directory contains data for services provided by this system.

## `/tmp`

The `/tmp` directory contains temporary files. It is a place for programs to store temporary files.

# Notes

1. Command binaries that are not essential enough to be placed in `/bin` are placed in `/usr/bin`
2. It is recommended that files to be stored in subdirectories of `/etc` rather than directly in `/etc`

# `/usr`

The `/usr` directory contains the secondary hierarchy of the system. It contains the majority of user utilities and applications.

the following directories are found in the `/usr` directory:

| Directory | Description |
| --------- | ----------- |
| `/usr/bin` | Non-essential command binaries |
| `/usr/include` | Header files included by C programs |
| `/usr/lib` | Libraries for the binaries in `/usr/bin` and `/usr/sbin` |
| `/usr/local` | Local hierarchy for locally installed software |
| `/usr/sbin` | Non-essential system binaries |
| `/usr/share` | Architecture-independent data |
| `/usr/src` | Source code (optional) |
| `/usr/games` | Games (optional) |

## `/usr/bin`

The `/usr/bin` directory contains non-essential command binaries. It contains the majority of user utilities and applications.

## `/usr/include`

The `/usr/include` directory contains header files included by C programs. It is used by the C compiler to find the header files.

## `/usr/lib`

The `/usr/lib` directory contains object files and libraries for the binaries in `/usr/bin` and `/usr/sbin`

## `/usr/sbin`

The `/usr/sbin` directory contains non-essential system binaries. It contains the non-essential system administration binaries.

# `/var`

The `/var` directory contains variable data. It contains files that are expected to grow in size or are modified frequently.
