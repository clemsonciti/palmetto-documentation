---
title: How to install your own software
keywords: [install,software]
sidebar: documentation_sidebar
permalink: userguide_howto_install_software.html
---

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

As an example, here are the steps for
installing [GROMACS](http://www.gromacs.org/),
a molecular simulation package:

1.  **Download GROMACS** You can usually download a software package to your laptop,
and then transfer the downloaded package to your `/home/username` directory on 
Palmetto for installation. Alternatively, if you have the http or ftp address for the package, you can transfer 
that package directly to your home directory while logged-in to Palmetto using the `wget` utility:

~~~
$ wget ftp://ftp.gromacs.org/pub/gromacs/gromacs-4.5.5.tar.gz
~~~

2.  **Unzip and unpack the `.tar.gz` file**:
Most software packages are compressed in a `.zip`, `.tar` or `.tar.gz` file.
You can use the `unzip` or `tar` programs to unpack the contents of these files:

~~~
