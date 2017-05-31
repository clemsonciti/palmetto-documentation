---
title: HOOMD-Blue
keywords: [HOOMD,hoomd]
sidebar: documentation_sidebar
permalink: software_hoomd.html
---



[HOOMD](http://codeblue.umich.edu/hoomd-blue/) (HOOMD-Blue) performs general purpose particle 
dynamics simulations on a single cluster node, 
taking advantage of NVIDIA GPU accelerators to attain a level of performance equivalent to many processor 
cores on a traditional large cluster.  HOOMD is not currently setup as a software module on Palmetto, 
but below are the steps that users can follow to install and run it.

First, HOOMD must be compiled using a node with NVIDIA GPUs (preferably the Tesla m2075), so you'll 
need to start an interactive session on an approriate GPU node:

    qsub -I -l select=1:ncpus=16:ngpus=2:gpu_model=m2075,walltime=2:00:00

Once your interactive session begins, load the software modules you'll need for compiling 
(and running) HOOMD.  Here, I'm using the GCC-4.4 compiler suite (the `gcc/4.4` module), but you can 
use your own build of GCC compilers.

    module add gcc/4.4 cuda-toolkit/5.0.35 cmake/2.8.7

Download the latest version of HOOMD (at the time I wrote this, 0.11.3 was the latest):

    wget http://codeblue.umich.edu/hoomd-blue/downloads/0.11/hoomd-0.11.3.tar.bz2

Uncompress the file you downloaded and setup a build directory (a separate, temporary directory 
where you can compile the HOOMD source code):

    tar -jxf hoomd-0.11.3.tar.bz2
    mkdir hoomd-build
    cd hoomd-build

Now, use CMAKE to configure the build and proceed with installing HOOMD.  I've specified that I 
want this package to be installed within my `/home` directory (in the `/home/galen/hoomd/0.11.3-g44` 
directory, which doesn't exist yet):

    cmake ../hoomd-0.11.3 -DCMAKE_INSTALL_PREFIX=/home/galen/hoomd/0.11.3-g44 -DCMAKE_BUILD_TYPE=Release -DENABLE_CUDA=ON -DENABLE_OPENMP=ON -DSINGLE_PRECISION=ON
    make
    make install

Once installed, I can add a few "export" commands to the end of my `~/.bashrc` file so I'll be ready to 
run HOOMD every time I start a new shell (log-in to a node):

    export LD_LIBRARY_PATH=/home/galen/hoomd/0.11.3-g44/lib/hoomd/python-module:$LD_LIBRARY_PATH
    export PATH=/home/galen/hoomd/0.11.3-g44/bin:$PATH


Now, I'm ready to try a test run of HOOMD. Create a new HOOMD input file (a Python script) named 
`input.hoomd` and add the following text to that file:

{% highlight python %}
from hoomd_script import *
# create 100 random particles of name A
init.create_random(N=100, phi_p=0.01, name='A')

# specify Lennard-Jones interactions between particle pairs
lj = pair.lj(r_cut=3.0)
lj.pair_coeff.set('A', 'A', epsilon=1.0, sigma=1.0)

# integrate at constant temperature
all = group.all();
integrate.mode_standard(dt=0.005)
integrate.nvt(group=all, T=1.2, tau=0.5)

# run 10,000 time steps
run(10e3)
{% endhighlight %}


With that input file ready to use, launch HOOMD to run the test within your interactive session:

    [galen@node1665 hoomd-test]$ hoomd test.hoomd

Or, you can setup a PBS job script for running that HOOMD job in batch mode. Here's what that job 
script might look like:

{% highlight bash %}
#!/bin/bash
#PBS -N HOOMD
#PBS -l select=1:ncpus=16:ngpus=2:gpu_model=m2075:mem=62gb,walltime=00:30:00
#PBS -j oe

source /etc/profile.d/modules.sh
module purge
module add gcc/4.4 cuda-toolkit/5.0.35

cd $PBS_O_WORKDIR

hoomd test.hoomd
{% endhighlight %}

