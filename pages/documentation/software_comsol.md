---
title: COMSOL
keywords: [comsol,COMSOL]
sidebar: documentation_sidebar
permalink: software_comsol.html
---

COMSOL is an application for solving Multiphysics problems.
To see the available COMSOL modules on Palmetto:

~~~
$ module avail comsol

comsol/4.3b comsol/5.0  comsol/5.1  comsol/5.2
~~~

To see license usage of COMSOL-related packages,
you can use the `lmstat` command:

~~~
/usr/flexlm/lmstat -a -c /usr/flexlm/licenses/comsol.dat
~~~

## Running COMSOL GUI

To run the COMSOL graphical interface,
you must [log-in with tunneling enabled]({{site.baseurl}}/userguide_howto_run_graphical_applications.html),
and then ask for an interactive session:

~~~
$ qsub -I -X -l select=1:ncpus=8:mpiprocs=8:mem=6gb:interconnect=mx,walltime=00:15:00
~~~

Once logged-in to an interactive compute node,
to launch the interactive viewer,
you can use the `comsol` command to run COMSOL:

~~~
$ comsol -np 8 -tmpdir $TMPDIR
~~~

The `-np` option can be used to specify the number of
CPU cores to use.
Remember to **always** use `$TMPDIR` as
the working directory for COMSOL jobs.

## Running COMSOL in batch mode

To run COMSOL in batch mode on Palmetto cluster,
you can use the example batch scripts below as a template.
The first example demonstrates running COMSOL using multiple cores
on a single node,
while the second demonstrates running COMSOL across multiple nodes
using MPI.
You can obtain the files required to run this example
using the following commands:

~~~
$ module add examples
$ example get COMSOL
$ cd COMSOL && ls

job.sh  job_mpi.sh
~~~

Both of these examples run the
"Heat Transfer by Free Convection" application described
[here](https://www.comsol.com/model/heat-transfer-by-free-convection-122).
In addition to the `job.sh` and `job_mpi.sh` scripts, to run the examples and reproduce the results,
you will need to download the file `free_convection.mph` provided
with the description.

### COMSOL batch job on a single node, using multiple cores:

~~~
#!/bin/bash
#PBS -N COMSOL
#PBS -l select=1:ncpus=8:mem=32gb,walltime=01:30:00
#PBS -j oe

module purge
module add comsol/5.2

cd $PBS_O_WORKDIR

comsol batch -np 8 -tmpdir $TMPDIR -inputfile free_convection.mph -outputfile free_convection_output.mph
~~~

### COMSOL batch job across several nodes

~~~
#!/bin/bash
#PBS -N COMSOL
#PBS -l select=2:ncpus=8:mpiprocs=8:mem=32gb,walltime=01:30:00
#PBS -j oe

module purge
module add comsol/5.2

cd $PBS_O_WORKDIR

uniq $PBS_NODEFILE > comsol_nodefile
comsol batch -clustersimple -f comsol_nodefile -tmpdir $TMPDIR -inputfile free_convection.mph -outputfile free_convection_output.mph
~~~

