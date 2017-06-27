---
title: R
keywords: [R,anaconda,conda]
sidebar: documentation_sidebar
permalink: programmers_r.html
---

## R versions available on Palmetto

To identify the version of the current R software, you can run R with the *--version*
flag. The version of the default R software on Palmetto is:

```bash
$ module list
No Modulefiles Currently Loaded.

$ R --version
R version 2.13.0 (2011-04-13)
Copyright (C) 2011 The R Foundation for Statistical Computing
ISBN 3-900051-07-0
Platform: x86_64-unknown-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License version 2.
For more information about these matters see
http://www.gnu.org/licenses/.
```

The "default" version of R on the Palmetto cluster
is **2.13.0**, released in 2011. R has undergone 
significant updates since then, and in general,
it is highly recommended that users **not** rely 
on this version of R to run their own applications:

1. Newer libraries will require other versions
of R, especially with R's transition to major 
revision 3.0 in 2013 to support 64-bit calculations.

2. The system version of R may change.
Any applications or packages that users
may build using the system R
will likely not run if this happens.

### R from Modules

R is also available on Palmetto under the form of 
software modules. These are copies of different versions
of R that are installed and configured for the cluster and
can be activate via Palmetto's module capability. 

```bash
$ module avail R
R/3.0.2 R/3.2.2

$ module load R/3.0.2
$ R --version
R version 3.0.2 (2013-09-25) -- "Frisbee Sailing"
Copyright (C) 2013 The R Foundation for Statistical Computing
Platform: x86_64-unknown-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License versions 2 or 3.
For more information about these matters see
http://www.gnu.org/licenses/.
```

While it is possile to use the R modules on Palmetto, 
the versions of R listed here are not up to date, and the rate of
new releases from R (current version 3.3.1) makes it 
difficult to maintain numerous versions
of R in a shared computing environment like Palmetto.

### Installing your own version of R

It is also possible for users to build, configure, and install
customized version of R from source. To do so, the following
steps must be taken:


- When building software, always ask for an interactive job first:

```bash
$ qsub -I -l select=1:ncpus=8:mem=4gb,walltime=4:00:00
```

- Create a temporary installation directory

```bash
$ cd $HOME
$ mkdir tmp
$ mkdir -p software/packages
```

- Dependency Installation: 

```bash
$ cd $HOME/tmp
$ wget http://zlib.net/zlib-1.2.11.tar.gz
$ wget http://www.bzip.org/1.0.6/bzip2-1.0.6.tar.gz
$ wget http://tukaani.org/xz/xz-5.2.2.tar.gz
$ wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.40.tar.gz
$ wget https://www.openssl.org/source/old/1.0.0/openssl-1.0.0k.tar.gz
$ wget --no-check-certificate https://curl.haxx.se/download/curl-7.47.1.tar.gz
$ tar xzf zlib-1.2.11.tar.gz
$ tar xzf bzip2-1.0.6.tar.gz
$ tar xzf xz-5.2.2.tar.gz
$ tar xzf pcre-8.40.tar.gz
$ tar xzf openssl-1.0.0k.tar.gz
$ tar xzf curl-7.47.1.tar.gz
$ cd zlib-1.2.11
$ ./configure --prefix=$HOME/software/packages
$ make
$ make install
$ cd ../bzip2-1.0.6
$ make -f Makefile-libbz2_so
$ make clean
$ make
$ make -n install PREFIX=$HOME/software/packages
$ make install PREFIX=$HOME/software/packages
$ cd ../xz-5.2.2
$ ./configure --prefix=$HOME/software/packages
$ make -j3
$ make install
$ cd ../pcre-8.40
$ ./configure --enable-utf8 --prefix=$HOME/software/packages
$ make -j3
$ make install
$ cd ../openssl-1.0.0k
$ ./config --prefix-$HOME/software/packages --openssldir=$HOME/software/openssl shared
$ make
$ make install
$ cd ../curl-7.47.1.tar.gz
$ ./configure ./configure --prefix=$HOME/software/packages --with-ssl=$HOME/software/openssl
$ make
$ make install
$ export PATH=$HOME/software/packages/bin:$PATH
$ export LD_LIBRARY_PATH=$HOME/software/packages/lib:$LD_LIBRARY_PATH 
$ export CFLAGS="-I$HOME/software/packages/include" 
$ export LDFLAGS="-L$HOME/software/packages/lib"
```

- Download the selected R source code into a temporary location in your home directory 
on Palmetto and install R into /home/yourusername/software/R/3.4.0

```bash
$ cd $HOME/tmp
$ wget https://cran.r-project.org/src/base/R-3/R-3.4.0.tar.gz
$ tar xzf R-3.4.0.tar.gz
$ cd tmp/R-3.4.0
$ ./configure --prefix=/home/yourusername/software/R-3.4.0
$ make
$ make install
```

- To use this location installation of R, path to its 
executables must be exported:

```bash
$ export R_HOME=/home/yourusername/software/R-3.4.0
$ export PATH=$R_HOME/bin:$PATH
$ R                                                                    
                                                                                                
R version 3.4.0 (2017-04-21) -- "You Stupid Darkness"                                           
Copyright (C) 2017 The R Foundation for Statistical Computing                                   
Platform: x86_64-pc-linux-gnu (64-bit)                                                          
                                                                                                
R is free software and comes with ABSOLUTELY NO WARRANTY.                                       
You are welcome to redistribute it under certain conditions.                                    
Type 'license()' or 'licence()' for distribution details.                                       
                                                                                                
  Natural language support but running in an English locale                                     
                                                                                                
R is a collaborative project with many contributors.                                            
Type 'contributors()' for more information and                                                  
'citation()' on how to cite R or R packages in publications.                                    
                                                                                                
Type 'demo()' for some demos, 'help()' for on-line help, or                                     
'help.start()' for an HTML browser interface to help.                                           
Type 'q()' to quit R.      
```

### R From Python Anaconda

To create a robust environment that can support
multiple versions of R, the Palmetto 
cluster leverages Anaconda, 
Python's package and environment manager, to 
allow users to take control of the set up process. 

The only prerequisite for this approach is to 
load one of the Python Anaconda modules available on
Palmetto:

```bash
$ module avail anaconda
anaconda/1.9.1  anaconda/2.3.0  anaconda/2.4.0  anaconda/2.5.0  anaconda/4.0.0  anaconda3/2.5.0 anaconda3/4.0.0

$ module add anaconda3/2.5.0

$ python --version
Python 3.5.2 :: Anaconda 2.5.0 (64-bit)

$ which python
/software/anaconda3/2.5.0/bin/python
```

Currently, a conda environment containing R 3.3.1
is already set up for Anaconda3/4.2.0, and this is 
also the default R kernel for Palmetto's JupyterHub

```bash
$ module load anaconda3/4.2.0

$ source activate R
(R) [lngo@node0042 ~]$ R --version
R version 3.3.1 (2016-06-21) -- "Bug in Your Hair"
Copyright (C) 2016 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License versions 2 or 3.
For more information about these matters see
http://www.gnu.org/licenses/.
```
