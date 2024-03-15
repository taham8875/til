# Top linux commands

# `cat`

The `cat` command is used to concatenate and display the content of files. It can be used to display the content of a file without opening it in a text editor.

```bash
cat [options] [file(s)]
```

## Create a new file

To create a new file, you can use the `cat` command with the `>` operator:

```bash
cat > file.txt
```

This will create a new file called `file.txt` and you can start typing in it. To save the file, press `Ctrl + D`.

```bash
$ cat > file.txt
Hello, this is a new file
This is the second line
^D
$
```

## Display the content of a file

To display the content of a file, you can use the `cat` command with the file name as an argument:

```bash
cat file.txt
```

This will display the content of the file `file.txt`:

```bash
$ cat file.txt
Hello, this is a new file
This is the second line
$
```

## Display line numbers

To display the content of a file with line numbers, you can use the `-n` option:

```bash
cat -n file.txt
```

This will display the content of the file `file.txt` with line numbers:

```bash
$ cat -n file.txt
     1  Hello, this is a new file
     2  This is the second line
$
```

## Redirect output to a file

To redirect the output of the `cat` command to a file, you can use the `>` operator:

```bash
$ cat file.txt > newfile.txt
$ cat newfile.txt
Hello, this is a new file
This is the second line
```

If the file does not exist, it will be created. If the file already exists, it will be overwritten.

> this also works for multiple files:
>
> ```bash
> $ cat file1.txt file2.txt > newfile.txt
> ```

## Display in reverse order

To display the content of a file in reverse order, you can use the `tac` command:

```bash
$ tac file.txt
This is the second line
Hello, this is a new file
```

## Append to a file

To append the output of the `cat` command to a file, you can use the `>>` operator:

```bash
$ cat file.txt >> newfile.txt
$ cat newfile.txt
Hello, this is a new file
This is the second line
Hello, this is a new file
This is the second line
```

or you can input the content via the command line directly:

```bash
$ cat >> newfile.txt
This is the third line
^D
$ cat newfile.txt
Hello, this is a new file
This is the second line
Hello, this is a new file
This is the second line
This is the third line
```

## `more` and `less` commands

The `more` and `less` commands are used to display the content of a file one page at a time. They are useful for viewing large files.

```bash
cat file.txt | more
```

```bash
cat file.txt | less
```

> `more` vs `less`: `less` is more powerful than `more`, `less` allows you to scroll up and down, but `more` only allows you to scroll down.

## Common `cat` options

- `-n`: Display line numbers
- `-e`: Display `$` at the end of each line
- `-t`: Display tabs as `^I`
- `-s`: Squeeze multiple blank lines into one
- `--help`: Display help message

# `chgrp`

The `chgrp` command is used to change the group ownership of a file or directory.

```bash
chgrp [options] [group] [file]
```

## Change group ownership

First, to display the group ownership of a file or directory, you can use the `ls` command with the `-l` option:

```bash
$ ls -l
taha@taha-Dell-G15-5511:~/til/linux$ ls -l
total 12
-rw-rw-r-- 1 taha taha 4192 Mar 14 21:50 'linux filesystem hierarchy standard.md'
-rw-rw-r-- 1 taha taha 3459 Mar 14 21:53 'top commands.md'
```

To change the group ownership of a file or directory, you can use the `chgrp` command with the group name and file name as arguments:

```bash
chgrp groupname 'top commands.md'
```

This will change the group ownership of the file `top commands.md` to `groupname`.

## Recursive change

To change the group ownership of a directory and all its contents recursively, you can use the `-R` option:

```bash
chgrp -R groupname directory
```

This will change the group ownership of the directory `directory` and all its contents to `groupname`.

## Common `chgrp` options

to see a list of common `chgrp` options, you can use the `--help` option:

```bash
taha@taha-Dell-G15-5511:~/til/linux$ chgrp --help
Usage: chgrp [OPTION]... GROUP FILE...
  or:  chgrp [OPTION]... --reference=RFILE FILE...
Change the group of each FILE to GROUP.
With --reference, change the group of each FILE to that of RFILE.

  -c, --changes          like verbose but report only when a change is made
  -f, --silent, --quiet  suppress most error messages
  -v, --verbose          output a diagnostic for every file processed
      --dereference      affect the referent of each symbolic link (this is
                         the default), rather than the symbolic link itself
  -h, --no-dereference   affect symbolic links instead of any referenced file
                         (useful only on systems that can change the
                         ownership of a symlink)
      --no-preserve-root  do not treat '/' specially (the default)
      --preserve-root    fail to operate recursively on '/'
      --reference=RFILE  use RFILE's group rather than specifying a
                         GROUP value
  -R, --recursive        operate on files and directories recursively

The following options modify how a hierarchy is traversed when the -R
option is also specified.  If more than one is specified, only the final
one takes effect.

  -H                     if a command line argument is a symbolic link
                         to a directory, traverse it
  -L                     traverse every symbolic link to a directory
                         encountered
  -P                     do not traverse any symbolic links (default)

      --help     display this help and exit
      --version  output version information and exit

Examples:
  chgrp staff /u      Change the group of /u to "staff".
  chgrp -hR staff /u  Change the group of /u and subfiles to "staff".

GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
Full documentation <https://www.gnu.org/software/coreutils/chgrp>
or available locally via: info '(coreutils) chgrp invocation'
```

# `chmod`

The `chmod` command is used to change the permissions of a file or directory.

```bash
chmod [options] mode file
```

example:

```bash
chmod 755 file
```

## common `chmod` options

- `-R`: change files and directories recursively
- `-v`: output a diagnostic for every file processed (verbose)
- `-c`: like verbose but report only when a change is made
- `-f`: suppress most error messages

## `chmod` mode

There are two ways to represent the mode:

### Symbolic mode

The symbolic mode is represented by three parts:

- `who`: the users to whom the permissions apply
- `op`: the operation to perform
- `perm`: the permissions to change

The `who` part can be one of the following:

| Symbol | Description |
| ------ | ----------- |
| `u`    | user        |
| `g`    | group       |
| `o`    | others      |
| `a`    | all (user, group, others) |

The `op` part can be one of the following:

| Symbol | Description |
| ------ | ----------- |
| `+`    | add permissions |
| `-`    | remove permissions |
| `=`    | set permissions to a specific value |

The `perm` part can be one or more of the following:

| Symbol | Description |
| ------ | ----------- |
| `r`    | read        |
| `w`    | write       |
| `x`    | execute     |

example:

1. this command adds the read, write and execute permissions for the user (file owner):

```bash
chmod u+rwx file
```

2. this command removes the write permission for group and others

```bash
chmod go-w file
```

### Octal mode

The octal mode is represented by a 3-digit number:

- First digit: user permissions
- Second digit: group permissions
- Third digit: others permissions

Each digit is the sum of the following values:

- 4: read
- 2: write
- 1: execute

In more verbose way, refer to the following table:

| value | permissions          | rwx |
| ----- | -------------------- | --- |
| 0     | none                 | 000 |
| 1     | execute only         | 001 |
| 2     | write only           | 010 |
| 3     | write, execute       | 011 |
| 4     | read only            | 100 |
| 5     | read, execute        | 101 |
| 6     | read, write          | 110 |
| 7     | read, write, execute | 111 |

example:

1. this command sets the read, write and execute permissions for the user, and read and execute permissions for the group and others:

```bash
chmod 755 file
```

### Practical example, how to make a script executable in linux

A famous command you may see when you search on how to make a script executable in linux is:

```bash
chmod +x script.sh
```

This command adds the execute permission for the user, group and others.

# `cp`

The `cp` command is used to copy files and directories.

```bash
cp [options] source destination
```

## common `cp` options

- `-r`: copy directories recursively
- `-v`: output a diagnostic for every file processed (verbose)
- `-i`: prompt before overwriting
- `-f`: force copy by removing the destination file if it exists
- `-u`: copy only when the source file is newer than the destination file or when the destination file is missing
- `-b`: create a backup of each existing destination file, the backup has `~` suffix unless a specified suffix is used via the `-S` option
- `-S`: specify a suffix for the backup file

and more, for complete guide on `cp` options, you can use the `--help` option:

## copy files

To copy a file, you can use the `cp` command with the source file and destination file as arguments:

```bash
cp file1 file2
```

This will copy the file `file1` to `file2`.

## copy directories

To copy a directory and all its contents, you can use the `-r` option:

```bash
cp -r directory1 directory2
```

This will copy the directory `directory1` and all its contents to `directory2`.

## copy multiple files

To copy multiple files to a directory, you can use the `cp` command with the source files as arguments and the destination directory as the last argument:

```bash
cp file1 file2 directory
```

This will copy the files `file1` and `file2` to the directory `directory`.

## use the wildcard character

You can use the wildcard character `*` to copy multiple files that match a pattern:

```bash
cp *.txt directory
```

This will copy all files with the `.txt` extension to the directory `directory`.

## create backup files

Copy file-1.txt to test-dir and create a backup file with the .bak extension. Show verbose output.

```bash
cp -v -b -S .bak file-1.txt test-dir
```

# `date`

The `date` command is used to display the current date and time.

```bash
date [options] [format]
```

## display the current date and time

To display the current date and time, you can use the `date` command without any arguments:

```bash
$ date
Thu Mar 14 11:36:35 PM EET 2024
```

The `-d` option can be used to display the date and time of a specific day:

```bash
$ date -d "2021-12-31"
Fri Dec 31 12:00:00 AM EET 2021
```

You can use the `+` operator to display the date and time in a specific format:

```bash
$ date +"%Y-%m-%d %H:%M:%S"
2024-03-14 23:36:35
$ date +"Current date: %Y-%m-%d, current time: %H:%M:%S"
Current date: 2024-03-14, current time: 23:36:35
```

## display past and future dates

You can use the `-d` option to display the date and time of a specific day:

```bash
$ date -d "yesterday"
Wed Mar 13 11:36:35 PM EET 2024
$ date -d "tomorrow"
Fri Mar 15 11:36:35 PM EET 2024
$ date -d "2 days ago"
Tue Mar 12 11:36:35 PM EET 2024
$ date -d "2 days"
Sat Mar 16 11:36:35 PM EET 2024
$ date -d "next monday"
Mon Mar 18 12:00:00 AM EET 2024
$ date -d "last year"
Tue Mar 14 11:36:35 PM EET 2023
```

## display the last modification time of a file

To display the last modification time of a file, you can use the `date` command with the `-r` option and the file name as an argument:

```bash
$ date -r file.txt
Thu Mar 14 11:36:35 PM EET 2024
```

## Display the local time in a different timezone

To display the local time in a different timezone, you can use the `date` command with the `-d` option and the timezone as an argument:

```bash
$ date -d "TZ=\"America/New_York\" 4:30 next Fri"
Fri Mar 15 10:30:00 AM EDT 2024
```

# `echo`

The `echo` command is used to display a line of text.

```bash
echo [options] [string]
```

## display a line of text

To display a line of text, you can use the `echo` command with the text as an argument:

```bash
$ echo Hello, world!
Hello, world!
```

## common `echo` options

to see a list of common `echo` options, you can use the `--help` option:

```bash
/bin/echo --help
```

the following options are available:

- `\n`: displays the output while omitting the newline after it
- `\E`: default, disable the interpretation of escape characters
- `-e`: enable the interpretation of escape characters:
  - `\a`: alert (bell)
  - `\b`: backspace
  - `\c`: omitting any further output
  - `\e`: escape
  - `\n`: newline
  - `\r`: carriage return
  - `\t`: horizontal tab
  - `\v`: vertical tab

example:

```bash
$ echo -e "Hello, \nworld!"
Hello,
world!
```

write to a file:

```bash
$ echo "Hello, world!" > file.txt
$ cat file.txt
Hello, world!
```

displaying variable values:

```bash
$ name="Taha"
$ echo "Hello, $name!"
Hello, Taha!
$ echo $USER
taha
```

display the output of an another command:

```bash
echo "this is a list of files and directories: $(ls)"
```

# `false`

The `false` command is used to return a non-zero exit status.

```bash
$ false
$ echo $?
1
```

> `echo $?` is used to display the exit status of the last command, `0` means success, and non-zero means failure.

# `true`

The `true` command is used to return a zero exit status.

```bash
$ true
$ echo $?
0
```

# `hostname`

The `hostname` command is used to display the name of the current host.

```bash
$ hostname
taha-Dell-G15-5511
```

# `ps`

The `ps` command is used to display information about processes.

```bash
ps [options]
```

## common `ps` options

- `-a`: display all processes
- `-u`: display detailed information about each of the processors
- `-x`: include displays that are controlled not by users but the daemons

for example, to display a detailed information about each of the processors:

```bash
ps -aux
```

# `pgrep`

The `pgrep` command is more complex than `ps`, it is used to display the process IDs of processes that match a pattern.

```bash
pgrep [options] pattern
```

for example, to display the process IDs of processes that match the pattern `firefox`:

```bash
pgrep firefox
```

## common `pgrep` options

- `-l`: display the process IDs and the names of the processes
- `-u`: display the process IDs of processes that are owned by a specific user
- `-x`: display the process IDs of processes that match the exact pattern
- `-o`: display the process ID of the oldest process that matches the pattern
- `-n`: display the process ID of the newest process that matches the pattern

for example, to list all the processor owned by root:

```bash
pgrep -l -u root
```

# `top`

The `top` command is the easiest and ui friendly way to display the list of processes and their usage.

```bash
top
```

> `top` enables you to kill a process interactively, by pressing `k` and entering the process ID.

# How to kill a process

Now after knowing how to get the process IDs, we can use the `kill` command to send a signal to a process.

killing a process sends a termination message to the given process, there are different signals that can terminate a process:

- `SIGKILL`: kills the process abruptly, it does not give the process a chance to clean up
- `SIGTERM`: kills the process gracefully, it gives the process a chance to clean up

## `killall` command

The `killall` command is used to kill processes by name. It sends a `SIGTERM` signal to the processes that match the given name.

```bash
killall [options] processname
```

## common `killall` options

- `-u`: kill processes that are owned by a specific user
- `-e`: find an exact match for the process name
- `-i`: prompt before killing each process
- `-v`: report back the processes that were killed
- `-o`: used with a duration to kill all processes that have been running for longer than the given duration
- `-y`: used with a duration to kill all processes that have been running for less than the given duration

for example, to kill all processes that are older than 1 hour:

```bash
killall -o 1h
```

# `pkill`

The `pkill` command is similar to `pgrep`, it is used to send a signal to a process that matches a pattern.

```bash
pkill [options] pattern
```

# `kill`

The `kill` command is used to send a signal to a process.

```bash
kill [options] pid
```

by default it sends a `SIGTERM` signal to the process, you can use the `-signal` option to send a different signal.

# `kill -9`

The `kill -9` command is used to kill an unresponsive process.

```bash
kill -9 pid
```

or

```bash
kill -SIGKILL pid
```

# `ls`

The `ls` command (short for list) is used to list the contents of a directory.

```bash
ls [options] [directory]
```

## common `ls` options

- `-F`: display a `/` after each directory and a `*` after each executable file (this helps to distinguish between files and directories)
- `-m`: display the contents as a comma-separated list
- `-Q`: display the contents with double quotes
- `-i`: display the inode number of each file
- `-r`: display the contents in reverse order
- `-t`: display the contents by modification time in order
- `-X`: display the contents by extension in alphabetical order
- `-a`: display all files and directories, including hidden ones
- `-l`: display detailed information about each file and directory
- `-R`: display the contents of subdirectories recursively

to get a full list of `ls` options, you can use the `--help` option:

```bash
ls --help
```

# `mkdir`

The `mkdir` command is used to create a new directory.

```bash
mkdir [options] directory
```

for example, to create a new directory called `test`:

```bash
mkdir test
```

you can make sure that the directory is made successfully via `ls` command

## create a directory in specific location

To create a new directory in a specific location, you can use the `mkdir` command with the `-p` option:

```bash
mkdir -p /path/to/directory
```

this will create the parent directories if they do not exist.

## create multiple directories

To create multiple directories, you can use the `mkdir` command with the directory names as arguments:

```bash
mkdir directory1 directory2 directory3
```

you can also use the curly braces `{}` to create multiple directories:

```bash
mkdir {directory1,directory2,directory3}
```

to automate creating multiple directories with a specific pattern, you can use also the curly braces `{}`:

```bash
$ mkdir dir{1..10}
$ ls
dir1  dir10  dir2  dir3  dir4  dir5  dir6  dir7  dir8  dir9 
```

## make directory with permissions

To create a new directory with specific permissions, you can use the `mkdir` command with the `-m` option:

```bash
mkdir -m 755 directory
```

for more on permission, refer to the `chmod` command.

# `mv`

The `mv` command is used to move (or rename) files and directories.

```bash
mv [options] source destination
```

## common `mv` options

- `-i`: prompt before overwriting
- `-f`: force move by removing the destination file if it exists
- `-v`: output a diagnostic for every file processed (verbose)
- `-i`: prompt before overwriting

## create backup files

Move file-1.txt to test-dir and create a backup file with the .bak extension. Show verbose output.

```bash
mv -S .bak -b name1 test-dir/name1
```

## batch rename files

combine `mv` with `for` loop to batch rename files:

```bash
for file in *.txt; do mv "$file" "new-$file"; done
```

# `pwd`

The `pwd` command is used to display the current working directory. You may either want to find the current directory or pass the current directory to a bash script.

```bash
pwd [options]
```

# `rm`

The `rm` command is used to remove files and directories.

```bash
rm [options] file
```

## common `rm` options

- `-i`: prompt before removing each file
- `-f`: force remove by ignoring nonexistent files and never prompt
- `-r`: remove directories and their contents recursively
- `-v`: output a diagnostic for every file processed (verbose)
- `-I`: prompt once before removing more than three files, or when removing recursively
- `-d`: remove empty directories

# `rmdir`

The `rmdir` command is used to remove empty directories.

```bash
rmdir [options] directory
```

## common `rmdir` options

- `-p`: remove the parent directories if they become empty after removing the directory
- `-v`: output a diagnostic for every directory processed (verbose)
- `-ignore-fail-on-non-empty`: ignore the failure if the directory is not empty
