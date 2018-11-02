---
title: LAMMPS
keywords: [lammps,LAMMPS,molecular dynamics]
sidebar: documentation_sidebar
permalink: software_lammps.html
---

There are a few different versions of LAMMPS available on the cluster,
but users are encouraged to install their own version of LAMMPS
in case newer versions different configurations are desired.

~~~
$ module avail lammps

lammps/10Jan15-dp     lammps/17Dec13-dp     lammps/17Dec13-dp-k20 lammps/29Aug14-sp-k20
~~~

## Installing LAMMPS on Palmetto cluster

In this example, we will demonstrate installing LAMMPS (version `22Aug18`).

1.  After logging in, ask for an interactive session (with GPU):

    ~~~
    $ qsub -I -l select=1:ncpus=8:mpiprocs=8:ngpus=1:mem=32gb:gpu_model=k40,walltime=2:00:00
    ~~~

2.  Load the required modules. Specifically, note that we are loading the CUDA-enabled
module `openmpi/1.10.3`:

    ~~~~
    $ module load gcc/5.4.0
    $ module load openmpi/1.10.3
    $ module load fftw/3.3.4-g481
    $ module load cuda-toolkit/8.0.44
    $ module load cmake/3.10.0    
    ~~~~

3.  Download the LAMMPS source code from http://lammps.sandia.gov/download.html.
The detailed instruction on usage and compilation options are available 
at http://lammps.sandia.gov/doc/Manual.html.

4.  Unpack the source code and enter the package directory:

    ~~~
    $ tar -xvf lammps-stable.tar.gz
    $ cd lammps-22Aug18
    ~~~
There are two different methods of compiling LAMMPS using **make** or **cmake**. We introduce both methods here:

5.  Compile LAMMPS using **make** with **KOKKOS**  support

    ~~~
    $ cd src
    $ make yes-kokkos 
    $ make mpi -j8 
    ~~~
    
    A new executable file **lmp_mpi** will be produced in the *src* folder. You can copy this executable over to the working directory of your jobs. Note that the size of the executable may be a hundred MB
    
    Note: there are optional packages which can be installed together with the above package using *make yes-*. For example: 
    ~~~
    $ make yes-USER-MISC
    $ make yes-KSPACE
    $ make yes-MOLECULE
    $ make yes-MISC
    $ make yes-manybody
    $ make yes-dipole
    $ make yes-class2
    $ make yes-asphere
    $ make yes-replica
    $ make yes-granular
    $ make yes-rigid
    ~~~

6.  Compile LAMMPS using **cmake** with **GPU** support:
The detailed manual can be found [here](https://github.com/lammps/lammps/blob/master/cmake/README.md) with more information on the options to be included. In this instruction, we only cover a certain number of options that are enough install cmake with GPU:

    ~~~
    $ mkdir build
    $ cd build
    $ cmake ../cmake/ -DPKG_GPU=on -DGPU_API=cuda -DGPU_PREC=double -DGPU_ARCH=sm_35 -DCUDA_CUDA_LIBRARY=/usr/lib64/nvidia/libcuda.so -DFFMPEG_EXECUTABLE=/software/ffmpeg/3.3.2/bin/ffmpeg
    $ make -j8
    ~~~

In the above script; the *-DGPU_ARCH=sm_35* was used because the gpu_model was set at Kepler (k40). Please refer to the above [guideline](https://github.com/lammps/lammps/blob/master/cmake/README.md) for setting particular gpu architecture

Note: there are optional packages which can be installed together with the above package using *-D[Options]=on*. For example: 

    ~~~
    $ cmake ../cmake/ -DPKG_MISC=on -DPKG_KSPACE=on -DPKG_MOLECULE=on -DPKG_MANYBODY=on -DPKG_DIPOLE=on -DPKG_CLASS2=on -DPKG_ASPHERE=on -DPKG_REPLICA=on -DPKG_GRANULAR=on -DPKG_RIGID=on
    ~~~
    
The executable `lmp` will be produced. You can copy this executable over to the working directory of your jobs.
Note that the size of the executable may be a hundred MB.

## Running LAMMPS - an example

This example will use the above compiled LAMMPS in a batch script.
The input file and batch script for this example can
be obtained using the following commands:

~~~
$ module add examples
$ example get LAMMPS

Getting example LAMMPS
Example LAMMPS copied into /home/username/LAMMPS.
~~~

Next, you will have to copy the above executable
`lmp` into the `LAMMPS` directory.
Examine the input file `in.lj`
and the batch script `job.sh` for this example.
The batch script looks as follows:

~~~
#PBS -N lammps_example
#PBS -l select=1:ncpus=10:mpiprocs=10:ngpus=1:mem=32gb,walltime=1:00:00
#PBS -j oe

module purge
module load gcc/5.4.0
module load openmpi/1.10.3
module load fftw/3.3.4-g481
module load cuda-toolkit/8.0.44
  

mpirun -n 8 lmp -k on t 8 g 1 -sf kk < in.lj
~~~
