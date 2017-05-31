---
title: How to run MPI applications
keywords: [license]
sidebar: documentation_sidebar
permalink: userguide_howto_use_mpi.html
---

For jobs using MPI,
an additional option `mpiprocs` must be specified
as part of the job limits specification
in the batch script (batch jobs) or `qsub` command (interactive jobs).
It is also a good idea to specify the interconnect required (`1g`, `10ge`, `mx`, or `fdr`).
Consult `/etc/hardware-table` for the available interconnects on different phases of the cluster.

Either one of the `gcc` or the `intel` modules
must be loaded prior to loading the `openmpi` modules.
Based on the requested interconnect
a specific implementation of the `openmpi` module will automatically
be loaded:

Interconnect                        | `openmpi` module loaded
------------------------------------|-------------------------
`1g` (1 Gbps ethernet)              | `-eth`
`10ge` (10 Gpbs ethernet)           | `-eth`
`10g` or `mx` (10 Gbps Myrinet)     | `-myri`
`fdr` or `56g` (56 Gbps Infiniband  | `-mlx*`

You do not have to explicitly load any of these specific `openmpi` modules.

For example, in interactive jobs:

~~~
qsub -I -l select=1:ncpus=8:mpiprocs=8:interconnect=mx,walltime=1:00:00
$ module add gcc/4.8.1 openmpi/1.8.1
$ module list
Currently Loaded Modulefiles:
  1) gmp/4.3.2            2) mpfr/2.4.2           3) mpc/0.8.1            4) gcc/4.8.1            5) openmpi/1.8.1-myri   6) openmpi/1.8.1

~~~

~~~
qsub -I -l select=1:ncpus=8:mpiprocs=8:interconnect=fdr,walltime=1:00:00
$ module add gcc/4.8.1 openmpi/1.8.1
$ module list
Currently Loaded Modulefiles:

  1) gmp/4.3.2           2) mpfr/2.4.2          3) mpc/0.8.1           4) gcc/4.8.1           5) openmpi/1.8.1-mlx   6) openmpi/1.8.1

~~~

And similarly in batch jobs:

~~~
#PBS -N mpi-exmaple
#PBS -l select=1:ncpus=8:mpiprocs=8:interconnect=fdr,walltime=1:00:00


...
~~~
