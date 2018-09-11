---
title: Singularity
keywords: [singularity,container,docker]
sidebar: documentation_sidebar
permalink: software_singularity.html
---

[Singularity](https://www.sylabs.io/)
is a tool for creating and running
[containers](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)
on HPC systems,
similar to [Docker](https://www.docker.com/).

For further information on Singularity,
and on downloading, building and running containers with Singularity,
please refer to the [Singularity documentation](https://www.sylabs.io/docs/).
This page provides information about singularity specific to the Palmetto cluster.

## Singularity module

It is recommended to use the following command to load the singularity module on Palmetto:

```
$ module load singularity
```

Because the version of singularity provided on Palmetto changes frequently,
it is **not** recommended to specify the version when loading the `singularity` module:

```
$ module load singularity/2.5.2     # not recommended
```

## Where to download containers

Containers can be downloaded from DockerHub

* [DockerHub](https://hub.docker.com/)
contains containers for various software packages,
and Singularity is
[compatible with Docker images](https://www.sylabs.io/guides/2.5/user-guide/singularity_and_docker.html).

* [SingularityHub](https://singularity-hub.org/)

* Many individual projects contain specific instructions for installation via
Docker and/or Singularity, and may host pre-built images in other locations.

## Example: Running OpenFOAM using Singularity

As an example, we consider installing and running the OpenFOAM CFD solver using Singularity.
OpenFOAM can be quite difficult to install manually,
but singularity makes it very easy.
This example shows how to use singularity interactively,
but singularity containers can be run in batch jobs as well.

Start by requesting an interactive job, and loading the singularity module.
Singularity can only be run on the compute nodes:

```
$ qsub -I -l select=1:ngpus=2:ncpus=16:mpiprocs=16:mem=120gb,walltime=5:00:00
$ module load singularity
```

We recommend that all users store built singularity images
in their `/home` directories.
Singularity images can be quite large,
so be sure to delete unused or old images:

```
$ mkdir ~/singularity-images
$ cd ~/singularity-images
```

Next, we download the singularity image for OpenFOAM from DockerHub.
This takes a few seconds to complete:

```
$ singularity pull docker://openfoam/openfoam6-paraview54
```

Once the image is downloaded,
we are ready to run OpenFOAM.
We use `singularity shell` to start a container,
and run a shell in the container.

The `-B` option is used to "bind" the `/scratch2/$USER` directory
to a directory named `/scratch` in the container.

We also the `--pwd` option to specify the working directory in the running container
(in this case `/scratch`).
This is **always** recommended.

Typically, the working directory may be the $TMPDIR directory or
one of the scratch directories.

```
$ singularity shell -B /scratch2/atrikut:/scratch --pwd /scratch openfoam6-paraview54.simg
```

Before running OpenFOAM commands, we need to source a few environment variables
(this step is specific to OpenFOAM):

```
$ source /opt/openfoam6/etc/bashrc 
```

Now, we are ready to run a simple example using OpenFOAM:

```
$ cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .
$ cd pitzDaily
$ blockMesh
$ simpleFoam
```

The simulation takes a few seconds to complete,
and should finish with the following output:

```
smoothSolver:  Solving for Ux, Initial residual = 0.00012056, Final residual = 7.8056e-06, No Iterations 6
smoothSolver:  Solving for Uy, Initial residual = 0.000959834, Final residual = 6.43909e-05, No Iterations 6
GAMG:  Solving for p, Initial residual = 0.00191644, Final residual = 0.000161493, No Iterations 3
time step continuity errors : sum local = 0.00681813, global = -0.000731564, cumulative = 0.941842
smoothSolver:  Solving for epsilon, Initial residual = 0.000137225, Final residual = 8.98917e-06, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 0.000215144, Final residual = 1.30281e-05, No Iterations 4
ExecutionTime = 10.77 s  ClockTime = 11 s


SIMPLE solution converged in 288 iterations

streamLine streamlines write:
    seeded 10 particles
    Tracks:10
    Total samples:11980
    Writing data to "/scratch/pitzDaily/postProcessing/sets/streamlines/288"
End
```

We are now ready to exit the container:

```
$ exit
```

Because the directory `/scratch` was bound to `/scratch2/$USER`, the simulation output is available in
the directory `/scratch2/$USER/pitzDaily/postProcessing/`:

```
$ ls /scratch2/$USER/pitzDaily/postProcessing/
sets
```
