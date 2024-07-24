# File descriptor

![File descriptors illustration](https://www.computerhope.com/jargon/f/file-descriptor-illustration.jpg)

A **file descriptor** is a number that uniquely identifies an open [file](https://www.computerhope.com/jargon/f/file.htm) in a computer's [operating system](https://www.computerhope.com/jargon/o/os.htm). It describes a data resource, and how that resource may be accessed.

When a program asks to open a file — or another data resource, like a [network socket](https://www.computerhope.com/jargon/n/network-socket.htm) — the [kernel](https://www.computerhope.com/jargon/k/kernel.htm):

1. Grants access.
2. Creates an entry in the global file table.
3. Provides the software with the location of that entry.

The descriptor is identified by a unique non-negative [integer](https://www.computerhope.com/jargon/i/integer.htm), such as 0, 12, or 567. At least one file descriptor exists for every open file on the system.

File descriptors were first used in [Unix](https://www.computerhope.com/jargon/u/unix.htm), and are used by modern operating systems including [Linux](https://www.computerhope.com/jargon/l/linux.htm), [macOS](https://www.computerhope.com/jargon/m/macosx.htm), and [BSD](https://www.computerhope.com/jargon/b/bsd.htm). In [Microsoft Windows](https://www.computerhope.com/jargon/w/windows.htm), file descriptors are known as [file handles](https://www.computerhope.com/jargon/f/filehand.htm).

- [Overview](https://www.computerhope.com/jargon/f/file-descriptor.htm#overview)
- [Stdin, stdout, and stderr](https://www.computerhope.com/jargon/f/file-descriptor.htm#std)
- [Redirecting file descriptors](https://www.computerhope.com/jargon/f/file-descriptor.htm#redirect)
- [Related information](https://www.computerhope.com/jargon/f/file-descriptor.htm#related)

## Overview

When a [process](https://www.computerhope.com/jargon/p/process.htm) makes a successful request to open a file, the kernel [returns](https://www.computerhope.com/jargon/r/return.htm) a file descriptor which points to an entry in the kernel's global file  table. The file table entry contains information such as the [inode](https://www.computerhope.com/jargon/i/inode.htm) of the file, byte [offset](https://www.computerhope.com/jargon/o/offset.htm), and the access restrictions for that [data stream](https://www.computerhope.com/jargon/d/datastre.htm) ([read-only](https://www.computerhope.com/jargon/r/readonly.htm), write-only, etc.).

![File descriptor diagram](https://www.computerhope.com/jargon/f/file-descriptor.jpg)

## Stdin, stdout, and stderr

On a Unix-like operating system, the first three file descriptors, by default, are STDIN ([standard input](https://www.computerhope.com/jargon/s/stdin.htm)), STDOUT (standard output), and STDERR (standard error).

| Name            | File descriptor | Description                                                  | Abbreviation |
| --------------- | --------------- | ------------------------------------------------------------ | ------------ |
| Standard input  | **0**           | The default data stream for input, for example in a command pipeline. In the [terminal](https://www.computerhope.com/jargon/t/terminal.htm), this defaults to keyboard input from the user. | **stdin**    |
| Standard output | **1**           | The default data stream for output, for example when a command prints text. In the terminal, this defaults to the user's screen. | **stdout**   |
| Standard error  | **2**           | The default data stream for output that relates to an error occurring. In the terminal, this defaults to the user's screen. | **stderr**   |

## Redirecting file descriptors

File descriptors may be directly accessed using [bash](https://www.computerhope.com/unix/ubash.htm), the default [shell](https://www.computerhope.com/jargon/s/shell.htm) of Linux, macOS X, and [Windows Subsystem for Linux](https://www.computerhope.com/jargon/w/wsl.htm).

For example, when you use the [find](https://www.computerhope.com/unix/ufind.htm) command, successful output goes to stdout (file descriptor 1), and  error messages go to stderr (file descriptor 2). Both streams display as terminal output:

```
find / -name '*something*'
/usr/share/doc/something
/usr/share/doc/something/examples/something_random
find: `/run/udisks2': Permission denied
find: `/run/wpa_supplicant': Permission denied
/usr/share/something
/usr/games/something
```

We're getting errors because find is trying to search a few system directories that we don't have [permission](https://www.computerhope.com/jargon/p/permissi.htm) to read. All the lines that say "Permission denied" were written to stderr, and the other lines were written to stdout.

You can hide stderr by [redirecting](https://www.computerhope.com/jargon/r/redirect.htm) file descriptor 2 to /dev/null, the special device in Linux that "goes nowhere":

```
find / -name '*something*' 2>/dev/null
/usr/share/doc/something
/usr/share/doc/something/examples/something_random
/usr/share/something
/usr/games/something
```

The errors sent to /dev/null, and are not displayed.

Understanding the difference between stdout and stderr is important when you want to work with a program's output. For example,  if you try to [grep](https://www.computerhope.com/unix/ugrep.htm) the output of the find command, you'll notice the error messages are  not filtered, because only the standard output is piped to grep.

```
find / -name '*something*' | grep 'something'
/usr/share/doc/something
/usr/share/doc/something/examples/something_random
find: `/run/udisks2': Permission denied
find: `/run/wpa_supplicant': Permission denied
/usr/share/something
/usr/games/something
```

However, you can redirect standard error to standard output, and then grep will process the text of both:

```
find / -name '*something*' 2>&1 | grep 'something'
/usr/share/doc/something
/usr/share/doc/something/examples/something_random
/usr/share/something
/usr/games/something
```

Notice that in the command above, the target file  descriptor (1) is prefixed with an ampersand ("&"). For more  information about data stream redirection, see [pipelines in the bash shell](https://www.computerhope.com/unix/ubash.htm#pipelines).

For examples of creating and using file descriptors in bash, see our [exec builtin command examples](https://www.computerhope.com/unix/bash/exec.htm#examples).







What is a file descriptor? When a process wants to communicate with a file, the kernel opens that file in memory and establishes a connection between the requesting process and the desired file through a File descriptor. File descriptor (which will be referred to as FD from now on) is actually an integer that is considered as an index for each of the files opened in the memory. At any moment there may be thousands of files open, each of which can be identified by its own FD. Kernel uses FD to transfer information: when a process wants to write new information in a file, it sends this information to the FD of that file (specific number of that file). From this number, the kernel determines which file it should transfer this information to. Also, when reading from the file, the kernel recognizes through this number to which process it should transfer the information read from the file. In this way, the kernel decides which direction to flow the bytes. Each process has parts that store the information it needs during its lifetime. One of these parts is the table for storing FDs. Every file that is opened by a process, its FD will be placed in this table by the kernel. When performing operations on the desired file, the process will refer to this table to obtain the FD of that file. Standard FDs Regardless of whether or not a process requests I/O during its processing, the kernel will by default create three FDs with the following numbers for all processes:     Number 0: standard input (stdin)     Number 1: standard output (stdout)     Number 2: standard error stream (stderr) These three standard FDs, which are created for all processes, allow us to treat the processes themselves as files! For example, if we want to transfer information to that process, we must send the data to the standard input of that process. Therefore, the "sending process" can imagine the "destination process" just like a file. With such a feature, in addition to I/O resources, we can deal with software resources such as Sockets, Pipes and many other resources as a file.