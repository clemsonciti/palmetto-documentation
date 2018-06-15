---
title: How to choose the appropriate filesystem (home, scratch, local_scratch)
keywords: [home,scratch,scratch1,scratch2,scratch3,local_scratch]
sidebar: documentation_sidebar
permalink: userguide_howto_choose_right_filesystem.html
---

The home directory is best for files/data that needs to be stored
**permanently**. The home directory should absolutely not be
used as the working directory for jobs.
It's acceptable for jobs to copy data from the home directory
to a scratch directory at the beginning of the job
and from scratch to home at the end of the job,
but applications should not read or write data to the home directory.

Some examples of data that is typically stored in the home directory: 

1. Code repositories, scripts, or notes written by the user
1. **compiled** programs/software
1. Final results from simulations/analyses

Some examples of data that is typically **not** stored in the home directory
(and are more suited to the *scratch* directories):

1. Intermediate data produced during simulations/analyses
1. Raw output that is to be processed to produce final results

Users may store unlimited amount of data in the scratch directories,
but data untouched for 30 days is automatically deleted and cannot
be recovered.

`/scratch2` and `/scratch3` are general purpose filesystems,
and will generally perform well for jobs:

- that read to or write from multiple files,
- where each file is read from and written by a single process, or
- where the majority of reads and writes are less than a few megabytes

`/scratch1` is a
[parallel file system](https://en.wikipedia.org/wiki/Clustered_file_system)
which will perform better for jobs
where multiple processes, across multiple nodes, read or write
the same file using parallel I/O routines.
The file sizes should typically be greater than a few megabytes
to see any performance benefit.
Several popular data formats such as
[HDF5](https://support.hdfgroup.org/HDF5/) and
[NetCDF](https://www.unidata.ucar.edu/software/netcdf/docs_rc/)
may be written to or read from efficiently in this manner.
Programs using MPI I/O will also be able to
efficiently read and write to `scratch1`.

The `/local_scratch` filesystem should **always** be preferred over the above scratch systems
unless the amount of data involved cannot fit in `local_scratch`.
The amount of `/local_scratch` varies by phase, and you can see how much each phase
has in `/etc/hardware-table`:

~~~
$ cat /etc/hardware-table
~~~

`local_scratch` will generally give the best performance compared
to the other filesystems. When using `local_scratch`, there are a few things to be kept in mind:

1.  Data should live in `local_scratch` only for the lifetime of the job: this means
that any data required by the job must be copied into `local_scratch` at the beginning
of the job, and any data required after job completion must be copied out of `local_scratch`
at the end of the job.

2.  `local_scratch` is shared between all users running processes on a node: because
two or more users may have running jobs on any given node,
they can all read or write to `local_scratch`. If - for any reason - you need
exclusive access to `local_scratch` on a node, you can schedule the *entire* node for yourself,
by specifying the `phase=` option in your job limits specification, and by requesting
the maximum number of cores for that phase, e.g.:

~~~
$ qsub -I -l select=1:ncpus=16:mem=4gb:phase=8b,walltime=1:00:00
~~~

3.  When using `local_scratch` in MPI (multi-node) jobs, each node will need to copy
data over to its `local_scratch`. To accomplish this, you can have commands similar to the
following in your batch script:

~~~

# beginning of job (copy data from home to local_scratch)

for node in `uniq $PBS_NODEFILE`
do
    ssh $node mkdir -p /local_scratch/username
    ssh $node cp /home/username/project/input_file /local_scratch/username
done

# ....other commands

# end of job (copy data from local_scratch to home)

for node in `uniq $PBS_NODEFILE`
do
    ssh $node cp /local_scratch/username/output_file /home/username/project
    ssh $node rm -r /local_scratch/username
done
~~~

`$PBS_NODEFILE` is a file containing
the nodes assigned to each chunk of hardware requested.
`uniq $PBS_NODEFILE` ensures that the list is reduced
only to unique nodes (two chunks may be on the same node).
