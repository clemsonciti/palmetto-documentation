---
title: Review of Linux
keywords: Linux
sidebar: documentation_sidebar
permalink: userguide_linux_review.html
---

Don't use Linux every day? A bit rusty? No problem, here's a brief review of some of the basics:

## Why Linux?

Linux is just another operating system (OS), like Microsoft's Windows or Apple's OSX,
but it is much more suited to performing tasks related to high-performance computing.
Although several Linux "flavours" such as Ubuntu and Linux Mint generally come with
graphical environments, the Palmetto cluster does not offer such an environment.
Instead, you will interact with the operating system via a simple command-line interface.
This is required because 
there's no room for the computational "overhead" of running a graphical user interface (GUI), 
especially not on a large HPC cluster with hundreds of users connecting to the system through 
remote network connections. In addition, the command-line makes it very easy to
automate tasks, and it can be much more efficient to work with the command line compared
to a graphical user interface.

Palmetto consists of hundreds of compute nodes (the nodes that do the computational work), 
plus a few service nodes that handle other activities. The most important service node is 
the "head" node, also called the log-in node. This node (named `login001`) is where you begin 
when you connect to Palmetto.

## When you log-in

See the [Logging in]({{site.baseurl}}/userguide_basic_usage.html#logging-in) section of the User's Guide
for instructions for logging in to the cluster.
When you log-in, you'll be presented with a system message called the "message of the day" (MOTD).
It looks something like this, with your command prompt waiting for you below this message:

~~~
 -----------------------------------------------------------------------------
       Welcome to the PALMETTO CLUSTER at CLEMSON UNIVERSITY

   * Email ithelp@clemson.edu with questions or to report problems.

   * Palmetto "office hours" are every Wednesday 8am-11am in 412 Cooper Library.

   * Quarterly maintenance periods:  May (followed by Top 500 benchmark),
     August, November and Feb.  Email will be sent before each period with
     details of cluster availability.

        User guide:  http://www.palmetto.clemson.edu/palmetto
   Sample programs:  https://github.com/clemsonciti/palmetto-examples
        JupyterHub:  https://www.palmetto.clemson.edu/jupyterhub

   Useful commands:
     module avail             - list available software packages
     qstat -xf jobid          - check status of your job
     qstat -Qf queuename      - check status of a queue
     checkquota               - check your disk quota
     checkqueuecfg            - check general workq max running limits
     cat /etc/hardware-table  - list node hardware: ram,cores,chip,etc.
     qpeek                    - look at a running job's stdout or stderr
     whatsfree                - see what nodes are free right now

   Please do not use /home as your PBS working directory.  Jobs with /home
   as working directory may be killed as performance deteriorates.

   DO NOT RUN JOBS/PROGRAMS/TESTS/PRE-OR-POST PROCESSING ON THE LOGIN NODE.
   They will be terminated without notice. No exceptions.

 -------------- This file is: /etc/motd -------- Last Updated: 21-FEB-2018 ----
~~~

Notice that the command prompt indicates what node (`login001`) you are currently connected to.
The `~` (tilde symbol) indicates that you are in your home directory `/home/username`. When you log-in to 
Palmetto, you will start in your home directory.
When logged-in to Palmetto, you're using the `bash` shell (a.k.a. the Bourne-Again shell).
The shell is a simple interface program that:

1. Reads (interprets) the command you type
2. Tries to make sense of it
3. Runs whatever programs your command needs
4. Prints the output of those programs, and then waits for another command

This is known as a Read-Evaluate-Print-Loop, or REPL.

##  Basic Commands

Try running some basic commands to see what the output looks like:

Command | Description
------- | -----------------
`pwd`   |  ("print working directory") prints your current working directory in the filesystem
`cal`   |  ("calendar") prints a little calendar for the current month
`date`  |  prints the current date and time
`ls`    |  list the contents of the directory you're in
`env`   |  list all environment variables/settings

Most commands accept or require arguments (a.k.a. "flags" or "options") that customize or 
specify how they work:

Command                                 | Description
--------------------------------------- | -------------
`cd /scratch1/username`                 |   ("change directory") in this example, changing to your directory in the `/scratch1` filesystem
`echo "Hello there, how are you?" `     |   prints the string of characters inside the "" quotes
`echo $PATH `                           |   print the value of the `PATH` variable
`which gcc`                             |   prints the full path to the `gcc` command (gcc is the GNU C compiler)
`cp compute.log /home/username/logfiles`|  copying the file `compute.log` to to the `logfiles` in the home directory

A manual page ("man page") serves as the "user manual page" for any particular command, 
including detailed information about how to use or customize each command:

~~~
$ man ls
~~~

From the man page for `ls`, we can see that the following command:

~~~
$ ls -latr
~~~

lists the contents of the current directory, all files (including hidden files),
and sorts them by time.

## Using a Text Editor

Because graphical applications cannot trivially be run on the Palmetto cluster,
you cannot use common text editors like Notepad, Atom, or Gedit to create and edit files.
Instead, you will have to use a text editor that runs entirely within the terminal
window. A very simple, easy-to-use terminal-based text editor is `nano`.
You can start `nano` and begin editing a filewith this command:

~~~
$ nano my-file.txt
~~~

At the bottom of the nano interface, you'll see a menu of common commands, such as `^O` 
(that's CTRL-o) to save or "write" a file that has been edited,
and `^X` (that's CTRL-x) to exit nano.

Working with Palmetto cluster will require you to frequently
create and edit text files. So it's important that you learn how to do this effectively.
There are other terminal-based text editors that you can use
that are much more powerful than nano, but take more effort to learn,
for example `vim` and `emacs`. Type `vimtutor` to start a short tutorial
on `vim`.

## I/O redirection

I/O redirection is a way of manipulating the input/output of Linux programs, allowing you to 
capture output in a file, or send it to another program.

The  `>`  character instructs bash to take the output of a command and write it to a file,
for example:

~~~
$ ls /scratch1/username > list.txt
~~~

writes the output of the `ls /scratch1/username` command  to `list.txt`.

Similarly, `>>` can be used to append to the end of a file
without overwriting what's already there:

~~~
$ echo "Right now it's `date`" >> list.txt
~~~

Another useful technique is to redirect one program's output (stdout)
into another program's input (stdin). This is done using a "pipe" character ( `|` ).
For example:

~~~
$ env
~~~

will print all of your current environment variables to the screen.
And

~~~
$ env | grep PBS
~~~

will send all of the `env` output to the `grep PBS` command,
and the result  will be a list of all environment variables that contain the string `PBS`.

## The .bashrc File

Every time you log in, the `.bashrc` script in your home directory is executed (note the dot `.` 
at the beginning of the name -- this means it's a hidden file, so use `ls -a` to see it).
You can add lines to the bottom of this file to run additional, custom commands every time you login.
For example, you can set a new environment variable
in your `.bashrc` script:

~~~
$ export MPI_HOME=/software/openmpi/1.8.4_gcc
~~~

If you log out and log back in, you can check that the variable $MPI_HOME is set:

~~~
$ echo $MPI_HOME
~~~

## File and Directory Permissions

Control access to files and directories by setting permissions (`r` = read, `w` = write, 
`x` = execute).  Use `ls -l` ("long listing") to see permissions settings:

~~~
[username@login001 ~]$ ls -al
total 3750128
drwx------ 102 username cuuser     143360 Mar 13 14:34 .
drwxr-xr-x 862 root  root        81920 Mar 13 13:51 ..
-rwx------   1 username cuuser        176 May 26  2010 .bash_profile
-rwx------   1 username cuuser       5667 Feb 27 10:54 .bashrc
-rwxr-xr-x   1 username staff      622783 Mar 13 14:33 dictionary.txt
-rwxr-xr-x   1 username staff      891777 Mar 13 14:34 personnel.txt
drwx------   2 username cuuser       4096 Jul  6  2010 petascale
drwx------   2 username cuuser       4096 Mar  5 15:14 tools
~~~

That first column of information listed here contains the permissions settings for each file 
or directory.  For example,

A permissions setting of  `-rwxr-xr-x` can be broken-down in this way:

~~~
-   rwx   r-x   r-x
~~~

`-`: The first character just indicates if this is a file (`-`) or a directory (`d`).

`rwx`:  The next 3 characters are the permissions of the file's owner, and here she has full 
`rwx` permissions.

`r-x`  The next 3 characters are the permissions for members of the owner's 
user group, and here they have only `r` and `x` permissions.

`r-x`: The last 3 characters are the 
permissions for all other users, and here they have only `r` and `x` permissions.

You can set or modify permissions settings using 3-digit octal notation, where: 

Digit | Permission
----- | ----------
0     | `---`
1     | `--x`
2     | `-w-`
3     | `-wx`	
4     | `r--`
5     | `r-x`
6     | `rw-`
7     | `rwx`

~~~
-rwxr-xr-x   1 username staff      622783 Mar 13 14:33 dictionary.txt
~~~

`chmod 740 dictionary.txt` With this command, you are changing permissions so that this file will be 
read-only (`r--`) for members of your group and other users will have no access (`---`).  Now, 
members of your group can read or copy this file, but they cannot modify, move, or delete the original:

    -rwxr-----   1 username staff      622783 Mar 13 14:38 dictionary.txt

##  Linux Commands "Quick Reference" Cheat Sheet

**File commands**

Command           | Description
----------------- | ----------------------------------
`ls`              | directory listing
`ls -al`          | formatted listing with hidden files
`cd dir`          | change directory (move) to `dir`
`cd`              | change to your `/home` directory
`pwd`             | show current directory
`mkdir dir`       | create directory named `dir`
`rm file`         | delete `file`
`rm -r dir`       | delete directory `dir`
`rm -f file`      | force deletion of `file`
`rm -rf dir`      | force deletion of `dir`
`cp file1 file2`  | copy `file1` to `file1`
`cp -r dir1 dir2` | copy `dir1` to `dir2`
`mv file1 file2`  | rename or move `file1` to `file2`
`mv file1 dir`    | move `file1` into `dir`
`ln -s file link` | create symbolic `link` to `file`
`touch file`      | create or update `file`
`cat > file`      | redirects stdin into `file`
`more file`       | output contents of `file`
`head -8 file`    | output first 8 lines of `file`
`tail -8 file`    | output last 8 lines of `file`
`tail -f file`    | output contents of `file` as it grows


**Process Management**

Command           | Description
----------------- | ----------------------------------
`ps`              | display your currently active processes
`top`             | display all running processes
`kill pid`        | kill process with ID number `pid`
`killall proc`    | kill all processes named `proc`
`bg`              | lists stopped or background processes
`fg`              | brings most recent process to foreground
`fg n`            | brings process `n` to the foreground


**File & Directory Permissions**

Command            | Description
------------------ | ----------------------------------
`chmod octal file` | change permissions of file (4 = r, 2 = w, 1 = x)
`chmod 777 file`   | read, write, execute for all
`chmod 700 file`   | `rwx` for owner only
`chmod 755 file`   | `rwx` for owner, `rx` for everyone else
`chown userid file`| change ownership of `file`
`chgrp group file` | change group ownership of `file`


**SSH & SCP**

Command                                | Description
-------------------------------------- | ----------------------------------
`ssh user@host`                        | connect to `host` as `user`
`ssh -p port user@host`                | connect using port number `port`
`ssh -X  user@host`                    | connect with `X11` forwarding
`scp file1 user@host:directory/file2`  | secure copy `file1` to `file2` on remote system
`scp user@host:directory/file1 file2`  | secure copy `file1` from remote system to `file2` on your local system

**Searching**

Command | Description
------- | -----------------------
`grep pattern file`  | search for pattern in file(s)
`grep -r pattern dir`  | search recursively for pattern in dir
`command \| grep pattern`  | search for pattern in output of command
`locate file`  | find all instances of file
`find . -name "file.txt"`  | recursively find all instances of file.txt starting in current directory

**System Info**

Command | Description 
------- | -----------------------------
`date` |  show current date and time
`cal` |  show current month's calendar
`who` |  display who is logged-in
`whoami` |  display username of current login
`uname -a` |  display OS and kernel version info
`cat /proc/cpuinfo` |  display CPU info
`cat /proc/meminfo` |  display memory info
`man command` |  display manual page for command
`df -h` |  show disk usage
`du` |  show directory space usage
`which app` |  show full path to app

**Compression**

Command | Description
------- | ----------------
`tar -cvf file.tar file(s)` | create a .tar file containing file(s)
`tar -xvf file.tar` |  extract contents of file.tar
`tar -cvfz` |  create .tar file with Gzip compression
`tar -zxvf` |  extract contents of a Gzip compressed .tar file
`gzip file` |  compresses file and renames it to file.gz
`gzip -d file.gz` |  decompresses file.gz back to file

**Installing Software**

Command | Description
------- | ----------------------------
`./configure` | run the configure script
`make` | build the software based on configure info
`make install` |  run the installation process
`rpm -Uvh package.rpm` | install an RPM package

**VI  and VIM**

Command | Description
------- | ------------------------
`vim file`  | edit file with VIM
`i`  | start "insert" mode (so you can edit the file)
`ESC` | return to "command" mode
`:q!` | force quit, from "command" mode
`:wq` | write (save) the file and quit, from "command"

**Miscellaneous**

Command | Description
------- | -------------------------------
`Ctrl+C` | halts the current command
`exit` | log-out of current session
`Ctrl+D` | log-out of current session (same as exit)
`!!` | repeats the last command
`cd -` | change directory to previous location

## Additional Useful Commands

Here are a few more "advanced" utilities that some users may find useful.

Print a list of directories and libraries available for linking (in this example, "name" 
could also be just part of the name):

~~~
ldconfig -p | grep name
~~~

Print all shared library dependencies for an executable (a way to see if the libraries you 
need to run that exe are available):

~~~
ldd myprogram.exe
~~~
