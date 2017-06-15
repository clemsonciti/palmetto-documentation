---
title: How to install your own software
keywords: [install,software,htop]
sidebar: documentation_sidebar
permalink: userguide_howto_install_software.html
---

## Things to keep in mind when installing software on Palmetto

When setting up your own software on Palmetto,
you should keep in mind the following:

* Package management systems like `yum`, or `apt-get`,
which are used to install software in typical
Linux systems are *not* available to
users on the Palmetto cluster. Instead, you will have
to either download pre-compiled executables (binaries)
or compile software yourself from source code.
Many software packages will provide documentation on how to
do this.

* Most software packages have dependencies, i.e.,
other packages that they depend on. These dependencies
may be compile-time (required only during compiling/installing),
run-time (required during running), or both.
You may either have to install these dependencies yourself
or they may be available as modules. For example,
many software packages will require a GNU C compiler
of some minimum version, and you can load the appropriate
`gcc` module before compiling such packages.

* Many installation instructions assume *root* access:
the documentation for most packages assume that you have
administrative priveleges on the computer that you
are installing to. You generally must tell the
package in some way to install itself into a directory
that you have permissions to write to,
like `/home/username`, rather than `/usr/local`.

## Example: `htop`

<img src="{{site.baseurl}}/images/htop.png" style="width:750px">

As an example,
we will walk through the process of installing
[`htop`](http://hisham.hm/htop/),
a command-line utility for monitoring
properties of running programs,
such as CPU and memory usage.

### Getting the source code

When you download a software package, it may be available
in the following forms:

1.  **Pre-compiled binaries/executables**: with pre-compiled software, you don't have to
do anything. The program can be run after downloading it.

2.  **Source code**: In the case, you will download the source code
for the program and compile it yourself. The type of compiler used
will depend on the language in which the program is written,
e.g., C, C++, Fortran, Java, etc.,

In the case of `htop`, we will be downloading its source code
and compiling it ourselves.

The source code for the latest version of `htop` can be downloaded
[here](http://hisham.hm/htop/releases/).
In this example, we will download [version 2.0.2](http://hisham.hm/htop/releases/2.0.2/).
Look for the `.tar.gz` file and download it to your home directory on Palmetto.
You can extract the contents of this file using the `tar` command:

~~~
$ tar -xvf htop-2.0.2.tar.gz
~~~

Once extracted, folder `htop-2.0.2` is created.

### Compiling and installing

To install `htop`, you can run the following commands
after entering the `htop-2.0.2` directory.
Remember to do compilation only on the compute nodes of the cluster:

~~~
$ qsub -I
$ cd htop-2.0.2

$ ./configure --prefix=/home/username/software
$ make
$ make install
~~~

The `configure` command checks if dependencies are installed,
and prepares the installation.
The `--prefix` option is required,
as otherwise the software will attempt to install into `/usr/local`,
and you will get a "Permission denied" error.

The `make` command compiles the source code and
creates the binary (or executable) `htop`;
and the `make instasll` command copies this binary
into the folder `/home/username/software/bin`.

### Running `htop`

You can run `htop` by specifying on the command-line
the full path to the `htop` binary:

~~~
$ /home/username/software/bin/htop
~~~

Alternatively, you can add the path `/home/username/software/bin`
to the `PATH` environment variable,
and then run just `htop`:

~~~
$ export PATH=$PATH:/home/username/software/bin
$ htop
~~~

To make this change to your `PATH` permanent,
you can add the `export` line to the end
of the file `/home/username/.bashrc`.

