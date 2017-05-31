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

In this example, we will demonstrate installing LAMMPS (version `17Nov16`) with support
for KOKKOS.

1. After logging in, ask for an interactive session (with GPU):

    ~~~
    $ qsub -I -l select=1:ncpus=8:mpiprocs=8:ngpus=1:mem=32gb,walltime=2:00:00
    ~~~

2.  Load the required modules. Specifically, note that we are loading the CUDA-enabled
module `openmpi/1.8.1`:

    ~~~~
    $ module load gcc/4.8.1
    $ module load openmpi/1.8.1
    $ module load fftw/3.3.4-g481
    $ module load cuda-toolkit/7.5.18
    ~~~~

3.  Download the LAMMPS source code from http://lammps.sandia.gov/download.html.
The detailed instruction on usage and compilation options are available 
at http://lammps.sandia.gov/doc/Manual.html.

4.  Unpack the source code and enter the package directory:

    ~~~
    $ tar -xvf lammps.tar.gz
    $ cd lammps-17Nov16
    ~~~

5.  Download Palmetto Makefile:

    ~~~
    $ PalmettoMakefile="https://raw.githubusercontent.com/zziolko/palmetto-software-installations/master/lammps/Makefile.palmetto_kokkos_cuda_openmpi"
    $ cd src/MAKE/MINE
    $ wget $PalmettoMakefile
    $ cd ../..
    ~~~

6. Compile LAMMPS:

    ~~~
    $ make yes-kokkos
    $ unset LIBRARIES
    $ make -j8 palmetto_kokkos_cuda_openmpi
    ~~~

The executable `lmp_palmetto_kokkos_cuda_openmpi` will be produced. You can
copy this executable over to the working directory of your jobs.
Note that the size of the executable may be a few hundred MB.

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
`lmp_palmetto_kokkos_cuda_openmpi` into the `LAMMPS` directory.
Examine the input file `in.lj`
and the batch script `job.sh` for this example.
The batch script looks as follows:

~~~
#PBS -N lammps_example
#PBS -l select=1:ncpus=10:mpiprocs=10:ngpus=1:mem=32gb,walltime=1:00:00
#PBS -j oe

module purge

module load gcc/4.8.1
module load openmpi/1.8.1
module load fftw/3.3.4-g481
module load cuda-toolkit/7.5.18

mpirun -n 8 lmp_palmetto_kokkos_cuda_openmpi -k on t 10 g 1 -sf kk < in.lj
~~~
