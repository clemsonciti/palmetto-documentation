---
title: GROMACS
keywords: [gromacs,GROMACS]
sidebar: documentation_sidebar
permalink: software_gromacs.html
---



Various installations of GROMACS are available on the cluster.
Different modules are provided for GPU-enabled and non-GPU versions
of GROMACS:

~~~
$ module avail gromacs

---------------------------------------------------------------- /software/modulefiles -----------------------------------------------------------------
gromacs/4.5.4-sp               gromacs/4.6.5-sp-k20-ompi      gromacs/5.0.1-sp-k20-g481-o181 gromacs/5.0.5-nogpu
gromacs/4.6.5-dp-ompi          gromacs/5.0.1-dp-g481-o181     gromacs/5.0.5-gpu

------------------------------------------------------------ /usr/share/Modules/modulefiles ------------------------------------------------------------
gromacs/4.5.4-sp
~~~

## Running GROMACS without GPU

To use the non-GPU version of GROMACS,
here is an example interactive job:

~~~
$ qsub -I -l select=2:ncpus=20:phase=10:mem=100gb:mpiprocs=20,walltime=1:
00:00
$ module load gromacs/5.0.5-nogpu
$ mpirun mdrun_mpi ...
~~~

where `...` specifies the input files and options of Gromacs.

And an example of a PBS batch script for submitting a GROMACS batch script:

~~~
#!/bin/bash
#PBS -l select=2:ncpus=20:phase=10:mem=100gb:mpiprocs=20
#PBS -l walltime=1:00:00

module load gromacs/5.0.5-nogpu
cd $PBS_O_WORKDIR

export OMP_NUM_THREADS=1

mpirun mdrun_mpi ...
~~~

## Running GPU-enabled GROMACS

For the GPU version

~~~
#!/bin/bash
#PBS -l select=2:ncpus=20:ngpus=2:phase=10:mem=100gb:mpiprocs=2
#PBS -l walltime=1:00:00

module load gromacs/5.0.5-gpu
cd $PBS_O_WORKDIR

export OMP_NUM_THREADS=10

mpirun mdrun_mpi ...
~~~

Note that the number of MPI processes per chunk is set as 2 when using the GPU.
Normally, the number of MPI processes is the same as the number of CPU cores requested.
But because there are only 2 GPUs per node, we launch only two MPI processes.
In the above example, each MPI process can also use 10 CPU cores
in addition to a GPU (this is enabled by setting the variable `OMP_NUM_THREADS` to 10).
