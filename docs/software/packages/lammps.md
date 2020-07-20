## LAMMPS

There are a few different versions of LAMMPS available on the cluster,
but users are encouraged to install their own version of LAMMPS
in case newer versions different configurations are desired.
In particular, there are two main components of LAMMPS which can be run with CPUs (lmp_mpi) or with GPUs (lmp)

~~~
$ module avail lammps

lammps/10Jan15-dp     lammps/17Dec13-dp     lammps/17Dec13-dp-k20 lammps/2018Dec12      lammps/29Aug14-sp-k20
~~~

### Installing LAMMPS on Palmetto cluster

In this example, we will demonstrate installing LAMMPS to run with CPUs and GPUs (version `7 Aug 2019`).
Detail information can be found here: https://lammps.sandia.gov/doc/Install.html

1.  After logging in, ask for an interactive session (with GPU):

    ~~~
    $ qsub -I -l select=1:ncpus=8:mpiprocs=8:ngpus=1:mem=64gb:gpu_model=v100,walltime=8:00:00
    ~~~

2.  Load the required modules. Specifically, note that we are loading the CUDA-enabled
module:

    ~~~~
    $ module load openmpi/3.1.4-gcc71-ucx fftw/3.3.4-g481 cuda-toolkit/9.2 cmake/3.13.1
    ~~~~

3.  Download the LAMMPS source code from http://lammps.sandia.gov/download.html.
The detailed instruction on usage and compilation options are available 
at http://lammps.sandia.gov/doc/Manual.html.

4.  Unpack the source code and enter the package directory:

    ~~~
    $ tar -xvf lammps-stable.tar.gz
    $ cd lammps-7Aug19
    ~~~
    
There are two different methods of compiling LAMMPS using **make** (support CPUs) or **cmake** (support GPUs). We introduce both methods here:

5.  Compile LAMMPS using **make** with **KOKKOS**  support

    ~~~
    $ cd src
    $ make yes-kokkos 
    $ make mpi -j8 
    ~~~
    
    A new executable file **lmp_mpi** will be produced in the *src* folder. You can copy this executable over to the working directory of your jobs. Note that the size of the executable may be a hundred MB
    
    Note: there are optional packages which can be installed together with the above package using *make yes-* prior to running *make mpi*. For example: 
    
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
    $ cmake ../cmake/ -DPKG_GPU=on -DGPU_API=cuda -DGPU_PREC=double -DGPU_ARCH=sm_70 -DCUDA_CUDA_LIBRARY=/usr/lib64/libcuda.so -DFFMPEG_EXECUTABLE=/software/ffmpeg/3.3.2/bin/ffmpeg -DPKG_MC=on -DPKG_REPLICA=on -DPKG_SNAP=on -DPKG-MANYBODY=on -DPKG_USER-MEAMC=on -DPKG_USER-MISC=on -DPKG_MISC=on
    $ make -j8
    ~~~

    In the above script; the *-DGPU_ARCH=sm_70* was used because the gpu_model was set at Volta (v100). Please refer to the above [guideline](https://github.com/lammps/lammps/blob/master/cmake/README.md) for setting particular gpu architecture

    Note: there are optional packages which can be installed together with the above package using *-D[Options]=on*. For example: 

    ~~~
    $ cmake ../cmake/ -DPKG_MISC=on -DPKG_KSPACE=on -DPKG_MOLECULE=on -DPKG_MANYBODY=on -DPKG_DIPOLE=on -DPKG_CLASS2=on -DPKG_ASPHERE=on -DPKG_REPLICA=on -DPKG_GRANULAR=on -DPKG_RIGID=on
    ~~~
    
The executable file **lmp** will be produced. You can copy this executable over to the working directory of your jobs.
Note that the size of the executable may be a hundred MB.

### Running LAMMPS - an example

Several existing examples are in the installed folder: *lammps-7Aug19/examples/*
Detailes description of all examples are [here](https://lammps.sandia.gov/doc/Examples.html#).
In order to run the example, simply copy the executable files created from step 5 and 6 to particular example folder and follow the **README** for detailed description on how to run.

For instance, in order to run an example *accelerate* using GPU package. Copy **lmp** to that folder.
Here is a sample batch script `job.sh` for this example:

~~~
#PBS -N accelerate 
#PBS -l select=1:ncpus=8:mpiprocs=8:ngpus=1:gpu_model=v100:mem=64gb,walltime=1:00:00
#PBS -j oe

module purge
module load openmpi/3.1.4-gcc71-ucx

mpirun -np 8 lmp -sf gpu < in.lj        # 8 MPI, 8 MPI/GPU
~~~
