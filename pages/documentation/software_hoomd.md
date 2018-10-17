---
title: HOOMD-Blue
keywords: [HOOMD,hoomd]
sidebar: documentation_sidebar
permalink: software_hoomd.html
---


## Installing HOOMD

[HOOMD](http://glotzerlab.engin.umich.edu/hoomd-blue/) (HOOMD-Blue) performs general purpose particle 
dynamics simulations on a single cluster node, 
taking advantage of NVIDIA GPU accelerators to attain a level of performance equivalent to many processor 
cores on a traditional large cluster.  HOOMD is not currently setup as a software module on Palmetto, 
but below are the steps that users can follow to install and run it.

1.	First, HOOMD must be compiled using a node with NVIDIA GPUs. Request an interactive session on a GPU node. For example:

    `$ qsub -I -l select=1:ncpus=16:mem=64gb:ngpus=2:gpu_model=p100:mpiprocs=16:interconnect=fdr,walltime=2:00:00`

2.	Load the required modules:

    `$ module add anaconda3/5.1.0 gcc/5.4.0 cuda-toolkit/9.0.176 cmake/3.10.0 openmpi/1.10.3 sqlite/3.21.0`

3.	Download the latest version (currently: v2.3.5) of HOOMD-BLUE to installed directory: (e.g: ~/source/)

    ```
    $ cd ~/source
    $ git clone --recursive https://bitbucket.org/glotzer/hoomd-blue
    ```

4.	A folder hoomd-blue is cloned to the *source* folder. Create *build* folder and go inside

    ```
    $ cd hoomd-blue
    $ mkdir build
    $ cd build
    ```

5.	Use CMAKE to configure the build and proceed with installing HOOMD to folder ~/applications

    ```
    $ cmake ../ -DCMAKE_INSTALL_PREFIX=~/applications/lib/python -DCMAKE_CXX_FLAGS=-march=native -DCMAKE_C_FLAGS=-march=native -DCMAKE_BUILD_TYPE=Release -DENABLE_CUDA=ON -DENABLE_MPI=ON -DSINGLE_PRECISION=ON
    $ make –j16 #Use 16 cores to compile faster
    $ make test #Optional
    $ make install  #This will create a hoomd folder in ~/applications/lib/python
    $ ccmake .
    press "c" to configure then press "g" to generate.
    ```

6.	Export the Python path to installed HOOMD folder. You might want to add this to bashrc file for faster load the HOOMD module:

    ```
    $ export PYTHONPATH=$PYTHONPATH:$HOME/applications/lib/python
    ```
    
## Run HOOMD
Now the HOOMD-BLUE v2.3.5 has been installed. Create a simple python file “test_hoomd.py” to run HOOMD 

```
import hoomd
import hoomd.md
hoomd.context.initialize("");
hoomd.init.create_lattice(unitcell=hoomd.lattice.sc(a=2.0), n=5);
nl = hoomd.md.nlist.cell();
lj = hoomd.md.pair.lj(r_cut=2.5, nlist=nl);
lj.pair_coeff.set('A', 'A', epsilon=1.0, sigma=1.0);
hoomd.md.integrate.mode_standard(dt=0.005);
all = hoomd.group.all();
hoomd.md.integrate.langevin(group=all, kT=0.2, seed=42);hoomd.analyze.log(filename="log-output.log",
                  quantities=['potential_energy', 'temperature'],
                  period=100,
                  overwrite=True);
hoomd.dump.gsd("trajectory.gsd", period=2e3, group=all, overwrite=True);
hoomd.run(1e4);
```

If you have logged out of the node, request an interactive session on a GPU node and add required modules:

```
$ qsub -I -l select=1:ncpus=16:mem=64gb:ngpus=2:gpu_model=p100:mpiprocs=16:interconnect=fdr,walltime=2:00:00
$ module add anaconda3/5.1.0 gcc/5.4.0 cuda-toolkit/9.0.176
```

* Run the script interactively:

`$ python test_hoomd.py`


* Alternatively, you can setup a PBS job script to run HOOMD in batch mode. A sample is below for *Test_Hoomd.sh*:


```
#PBS -N HOOMD
#PBS -l select=1:ncpus=16:mem=64gb:ngpus=2:gpu_model=p100:mpiprocs=16:interconnect=fdr,walltime=02:00:00
#PBS -j oe

module purge
module add anaconda3/5.1.0 gcc/5.4.0 cuda-toolkit/9.0.176

cd $PBS_O_WORKDIR
python test_hoomd.py
```

Submit the job:

`$ qsub Test_Hoomd.sh`
