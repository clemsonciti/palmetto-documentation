---
title: GROMACS
keywords: [gromacs,GROMACS]
sidebar: documentation_sidebar
permalink: software_gromacs.html
---

## GROMACS modules

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

### Running GROMACS without GPU

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

### Running GPU-enabled GROMACS

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

## GPU-enabled GROMACS using Singularity and NGC

The NVIDIA GPU cloud provides images for GPU-enabled GROMACS
that can be downloaded and run using Singularity on the Palmetto cluster.
This is the recommended way to run GROMACS on Palmetto.


### Downloading the image

Before downloading images from NGC,
you will need to obtain an NVIDIA NGC API key,
instructions for which can be found
[here](https://docs.nvidia.com/ngc/ngc-getting-started-guide/index.html).

Start an interactive job,
and create a directory for storing Singularity images
(if you don't have one already):

```
$ qsub -I -l select=1:ngpus=2:ncpus=16:mem=20gb:gpu_model=p100,walltime=5:00:00
$ mkdir -p singularity-images
```

Set the required environment variables (you will need the API key for this step):

```
$ export SINGULARITY_DOCKER_USERNAME='$oauthtoken'
$ export SINGULARITY_DOCKER_PASSWORD=<NVIDA NGC API key>
```

Navigate to the `singularity-images` directory and pull
the GROMACS image (choose from the images listed
[here](https://ngc.nvidia.com/registry/hpc-gromacs):

```
$ cd ~/singularity-images
$ module load singularity
$ singularity pull docker://nvcr.io/hpc/gromacs:2016.4
```

The above should take a few seconds to complete.

### Running GROMACS interactively

As an example,
we'll consider running the GROMACS ADH benchmark.

First, request an interactive job:

```
$ qsub -I -l select=1:ngpus=2:ncpus=16:mpiprocs=16:mem=120gb:gpu_model=p100,walltime=5:00:00
```

Load the singularity module:

```
$ module load singularity
```

Prepare the input and output directories:

In this case,
we use `/scratch3/$USER/gromacs_ADH`
as both the input directory (containing the data)
and the output directory (where output will be stored).

Remember that `$TMPDIR` is cleaned up after the job completes,
so it's important to move any data out of `$TMPDIR` after GROMACS completes running.
Also, in this case, we are downloading the input data from the web,
but if your input is stored in the `/home`/, `/scratch` or some other directory,
then you can copy it to `$TMPDIR`:

```
$ mkdir -p /scratch3/$USER/gromacs_ADH_benchmark
$ cd /scratch3/$USER/gromacs_ADH_benchmark
$ wget ftp://ftp.gromacs.org/pub/benchmarks/ADH_bench_systems.tar.gz
$ tar -xvf ADH_bench_systems.tar.gz
```

Use `singularity shell` to interactively run a container
from the downloaded GROMACS image.
The `-B` switch is used to "bind" directories on the host (Palmetto compute node)
to directories in the container.
The different bindings used are:

* `/scratch3/$USER/gromacs_ADH_benchmark` is bound to `/input`
* `/scratch3/$USER/gromacs_ADH_benchmark` is bound to `/output`
* `$TMPDIR` is bound to `/work`

```
$ singularity shell --nv \
    -B /scratch3/$USER/gromacs_ADH_benchmark:/input \
    -B /scratch3/$USER/gromacs_ADH_benchmark:/output \
    -B $TMPDIR:/work \
    --pwd /work \
    ~/singularity-images/gromacs-2016.4.simg \
```

Running the benchmark:

```
$ source /opt/gromacs/install/2016.4/bin/GMXRC
$ gmx_mpi grompp -f /input/adh_cubic/pme_verlet.mdp -c /input/adh_cubic/conf.gro -p /input/adh_cubic/topol.top 
$ export OMP_NUM_THREADS=8
$ mpirun -np 2 gmx_mpi mdrun -g adh_cubic.log -pin on -resethway -v -noconfout -nsteps 10000 -s topol.tpr -ntomp $OMP_NUM_THREADS
```

After the last command above completes,
the `.edr` and `.log` files produced by GROMACS should be visible.
Typically, the next step is to copy these results to the 
output directory:

```
$ cp *.log *.edr /output
$ exit
```
Upon exiting the container,
the `.log` and `.edr` files will be found in the output directory,
`/scratch3/$USER/gromacs_ADH_benchmark`.

### Running GROMACS in batch mode

The same benchmark can be run in batch mode by
encapsulating the commands in a script `run_adh.sh`,
and running it non-interactively using `singularity exec`.

```
# run_adh.sh

source /opt/gromacs/install/2016.4/bin/GMXRC
gmx_mpi grompp -f /input/adh_cubic/pme_verlet.mdp -c /input/adh_cubic/conf.gro -p /input/adh_cubic/topol.top 
export OMP_NUM_THREADS=8
mpirun -np 2 gmx_mpi mdrun -g adh_cubic.log -pin on -resethway -v -noconfout -nsteps 10000 -s topol.tpr -ntomp $OMP_NUM_THREADS
cp *.log *.edr /output
```

The PBS batch script for submitting the above
(assuming that the `/scratch3/$USER/gromacs_ADH_benchmark` directory already contains the input files):

```
#PBS -N adh_cubic
#PBS -l select=1:ngpus=2:ncpus=16:mem=20gb:gpu_model=p100,walltime=5:00:00

module load singularity

cd $PBS_O_WORKDIR

cp run_adh.sh $TMPDIR

singularity shell --nv \
    -B /scratch3/$USER/gromacs_ADH_benchmark:/input \
    -B /scratch3/$USER/gromacs_ADH_benchmark:/output \
    -B $TMPDIR:/work \
    --pwd /work \
    ~/singularity-images/gromacs-2016.4.simg bash run_adh.sh
```

