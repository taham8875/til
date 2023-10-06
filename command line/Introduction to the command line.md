# Introduction to the command line

**Shell**: A program that takes commands from the keyboard and gives them to the operating system to perform.

Basic commands:

To print a string:

```bash
$ echo "Hello World"
Hello World
```

To print the current user:

```bash
$ whoami
user
```

To print the location of the shell:

```bash
$ echo $0
-bash
```

> Note the linux is case sensitive, so `echo` is not the same as `Echo` or `ECHO`.

> Tip: use the up and down arrow keys to cycle through previous commands.

To see the history of commands:

```bash
$ history
1 echo "Hello World"
2 whoami
3 echo $0
4 history
```

## Package managers

A package manager is a collection of software tools that automates the process of installing, upgrading, configuring, and removing computer programs for a computer's operating system or a development platform in a consistent manner.

examples :

- npm
- apt
- brew
- pip

In linux we have a package manager called `apt` (Advanced Package Tool) that is used to install, remove and update software packages.

To see the available commands:

```bash
$ apt
apt 2.4.9 (amd64)
Usage: apt [options] command

apt is a commandline package manager and provides commands for
searching and managing as well as querying information about packages.
It provides the same functionality as the specialized APT tools,
like apt-get and apt-cache, but enables options more suitable for
interactive use by default.

Most used commands:
  list - list packages based on package names
  search - search in package descriptions
  show - show package details
  install - install packages
  reinstall - reinstall packages
  remove - remove packages
  autoremove - Remove automatically all unused packages
  update - update list of available packages
  upgrade - upgrade the system by installing/upgrading packages
  full-upgrade - upgrade the system by removing/installing/upgrading packages
  edit-sources - edit the source information file
  satisfy - satisfy dependency strings

See apt(8) for more information about the available commands.
Configuration options and syntax is detailed in apt.conf(5).
Information about how to configure sources can be found in sources.list(5).
Package and version choices can be expressed via apt_preferences(5).
Security details are available in apt-secure(8).
                                        This APT has Super Cow Powers.
```

Let's try to install `nano` (a text editor):

```bash
$ sudo apt install nano
```

if it cannot find the package, you can update the list of available packages:

```bash
$ sudo apt update
```

Now to make sure that `nano` is installed:

```bash
$ nano --version
GNU nano, version 6.2
```

To remove a package:

```bash
$ sudo apt remove nano
```

## File system

The file system is the way files are named, stored, and organized on a computer.

In linux, the file system is a tree structure with a root directory (`/`) at the top.

To see the current directory:

```bash
$ pwd
/home/user
```

To navigate to the root directory:

```bash
$ cd /
```

To list the files in the current directory:

```bash
$ ls
Docker  boot  etc   init  lib32  libx32      media  opt   root  sbin  srv  tmp  var
bin     dev   home  lib   lib64  lost+found  mnt    proc  run   snap  sys  usr
```

To navigate to the home directory:

```bash
$ cd ~
```

To navigate to the previous directory:

```bash
$ cd -
```

Navigate to the home directory again, and create a new directory called `test`:

```bash
$ cd ~
$ mkdir test
$ cd test
```

Make a new file called `test.txt`:

```bash
$ touch test.txt
```

You can rename a file using the `mv` command:

```bash
$ mv test.txt test2.txt
```

You can move a file to another directory using the `mv` command:

```bash
$ mv test2.txt /etc
```

## View and edit files

To write to a file:

```bash
$ echo "Hello World" > /etc/test2.txt
```

Or you can use a text editor like `nano`:

```bash
$ nano /etc/test2.txt
```

To view the content of a file:

```bash
$ cat /etc/test2.txt
```

If your file is too long, you can use `more` command:

```bash
$ more /etc/test2.txt
```

The problem with `more` is that you can only scroll down, you cannot scroll up. To solve this problem, we can use `less` command:

```bash
$ less /etc/test2.txt
```

## Redirection

Redirection is a way to send the output of a command to a file instead of the screen.

To append to a file:

```bash
$ echo "Hello World" >> /etc/test2.txt
```

To redirect the output of a command to a file:

```bash
$ ls -l > /etc/test2.txt
```

To redirect the content of a file to another file:

```bash
$ cat /etc/test2.txt > /etc/test3.txt
```
