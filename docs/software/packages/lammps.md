## LAMMPS

There are a few different versions of LAMMPS available on the cluster.

~~~
$ module avail lammps

------------------------- /software/ModuleFiles/modules/linux-centos8-x86_64 -------------------------
lammps/20190807-gcc/8.3.1-cuda10_2-mpi-openmp-user-omp
lammps/20200505-gcc/8.3.1-cuda10_2-kokkos-mpi-nvidia_P-openmp-user-omp
lammps/20200505-gcc/8.3.1-cuda10_2-kokkos-mpi-nvidia_K-openmp-user-omp
lammps/20200505-gcc/8.3.1-cuda10_2-kokkos-mpi-nvidia_V-openmp-user-omp (D)
~~~
Note: letter P, K, V stand for GPU pascal (p100), kepler (k20, k40) and volta (v100)

### Running LAMMPS - an example

Several existing examples are in the installed folder: *lammps-7Aug19/examples/*
Detailes description of all examples are [here](https://lammps.sandia.gov/doc/Examples.html#).

We run an example *accelerate* using different package
Here is a sample batch script `job.sh` for this example:

~~~
#PBS -N accelerate 
#PBS -l select=1:ncpus=8:mpiprocs=8:ngpus=2:gpu_model=v100:mem=64gb,walltime=4:00:00
#PBS -j oe

cd $PBS_O_WORKDIR
module purge
module load lammps/20200505-gcc/8.3.1-cuda10_2-kokkos-mpi-nvidia_V-openmp-user-omp

mpirun -np 8 lmp -in in.lj > output.txt        # 8 MPI, 8 MPI/GPU
# to write the output to a file
~~~

### Several way to run LAAMPS
## Running LAMMPS with KOKKOS packages
mpirun -np 8 lmp -k on -sf kk -in in.lj > output_kokkos.txt 

