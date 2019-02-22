---
title: openbabel
keywords: [openbabel]
sidebar: documentation_sidebar
permalink: software_openbabel.html
---

This page instructs how to install Open Babel software to Palmetto
The code and the issue tracker can be found [here](https://openbabel.org/docs/dev/Installation/install.html)

1. Request an interactive session. For example:

   ```
   $ qsub -I -l select=1:ncpus=6:mem=24gb:interconnect=fdr,walltime=3:00:00
   ```

1. Load the required modules

   ```
   $ module load cmake/3.6.1 boost/1.65.1 Qt/4.8.5 cuda-toolkit/9.0.176 gcc/7.1.0 
   ```

1. Install Eigen (latest version 3.3.7)

  ```
  $ cd ~/source
  $ wget http://bitbucket.org/eigen/eigen/get/3.3.7.tar.gz
  $ tar -xvf 3.3.7.tar.gz
  $ cd eigen-eigen-323c052e1731
  $ mkdir build
  $ cd build
  $ cmake -DCMAKE_INSTALL_PREFIX=$HOME/applications/ ..
  $ make install
  ```

1. Download Open Babel from [source](https://sourceforge.net/projects/openbabel/files/openbabel/2.4.1/openbabel-2.4.1.tar.gz/download).
Here we download the latest version 2.4.1.
Upload the package openbabel-2.4.1.tar.gz to your Palmetto storage
Unpack the downloaded file and go into openbabel source folder

   ```
   $ tar -xvf openbabel-2.4.1.tar.gz
   $ cd openbabel-2.4.1
   $ mkdir build
   $ cd build
   ```
   
1. Run cmake file

   ```
   $ cmake -DCMAKE_INSTALL_PREFIX=$HOME/applications/ -DLIB_INSTALL_DIR=$HOME/applications/lib -DEIGEN3_INCLUDE_DIR=$HOME/applications/include/eigen3/ -DRUN_SWIG=FALSE -DMINIMAL_BUILD=FALSE -DBUILD_GUI=FALSE .. 
   $ make -j4 #Using 4 cores to compile faster
   $ make install
   ```

1. The *obabel* executable file will be created in the installation folder: **/home/username/applications/bin**.
Make sure you set the correct environment PATH in ~/.bashrc file

   ```
   export PATH=$PATH:$HOME/applications/bin
   ```
