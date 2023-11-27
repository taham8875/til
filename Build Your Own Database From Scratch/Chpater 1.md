# Chapter 1 - Files vs Databases

This chapter shows the limitations of simply dumping data to files and the problems that databases solve.

## Persistance data to files

Storing data in files is simple, but it has many limitations and drawbacks:

- It truncates the file before writing, so if the process crashes while writing, the file will be corrupted.
- Writing data to files is not atomic, depending on the size of the write, concurrent readers might see inconsistent data.
- When is the data actually persisted to the disk? The data is probably still in the operating system’s page cache after the write syscall returns. What’s the state of the file when the system crashes and reboots?

## Atomic renaming

A simple solution is atomic renaming. We can write the data to a temporary file, and then rename the file to the final name. Renaming a file is atomic on most operating systems, so this solves the first problem.

Although this solution solves the first problem, it does not solve the persistence problem. The data is still in the operating system’s page cache, and it will be lost if the system crashes.

## fsync

To fix this problem, we need to call fsync. fsync forces the operating system to write the data to disk.

> The fsync() function is intended to force a physical write of data from the buffer cache, and to assure that after a system crash or other failure that all data up to the time of the fsync() call is recorded on the disk.

Do this solve the problem? Not really. What about metadata? What about the directory entry? What about the parent directory?

These approaches alone are not enough to build a database. A database uses additional `indexes` to make queries faster.
