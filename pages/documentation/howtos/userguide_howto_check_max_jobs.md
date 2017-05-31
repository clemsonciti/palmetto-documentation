---
title: How to check the maximum number of jobs that can be run
keywords: [license]
sidebar: documentation_sidebar
permalink: userguide_howto_check_max_jobs.html
---

When you submit a job,
it is forwarded to a specific **execution queue**
based on job critera
(e.g., how many cores, RAM, are needed, required walltime, etc.).
Broadly speaking there are three classes of execution queues:

1. MX queues: jobs submitted to run on the older hardware (phases 1-6)
will be forwarded to theses queues.

2. IB queues: jobs submitted to run the newer hardware (phases 6 and up)
will be forwarded to these queues.

3. bigmem queue: jobs submitted to the large-memory machines (phase 0).

Each execution queue has its own limits for
how many jobs can be running at one time,
and how many jobs can 
be waiting in that execution queue.
The maximum number of running jobs per user in
execution queues may vary
throughout the day depending on cluster load.
Users can see what the current limits
are using the `checkqueuecfg` command:

~~~
$ checkqueuecfg

                per job    per job   per user

MX QUEUES      min_cpus   max_cpus   max_jobs
c1_solo               1          1       5000
c1_single             2         24         50
c1_tiny              25        128         10
c1_small            129        512          2
c1_medium           513       2048          2
c1_large           2049       4096          1

IB QUEUES      min_cpus   max_cpus   max_jobs
c2_single             2         24          5
c2_tiny              25        128          3
c2_small            129        512          1
c2_medium           513       2048          1
c2_large           2049       4096          0

BIGMEM QUEUE   min_cpus   max_cpus   max_jobs
bigmem_e              1         64          1
~~~

The `qstat` command tells you which of the execution queues
your job is forwarded to. For example, here is an interactive
job requesting 8 CPU cores, a K40 GPU, and 32gb RAM:

~~~
$ qsub -I -l select=1:ncpus=8:ngpus=1:gpu_model=k40:mem=32gb,walltime=2:00:00
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 9567792.pbs02 to start
~~~

We see from `qstat` that the job request is forward to
the `c2_sing` queue:

~~~
[atrikut@login001 ~]$ qstat 9567792.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
9567792.pbs02     STDIN            atrikut                  0 Q c2_single
~~~

------------------------------------------------------------------------
