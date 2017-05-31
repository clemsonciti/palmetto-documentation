---
title: Quick Start
keywords: [login,submit,job,data,transfer]
sidebar: documentation_sidebar
permalink: userguide_quickstart.html
---



-----------------------------------------------------------------------

## Logging in

Any user with a Palmetto Cluster account can log-in using
[SSH (Secure Shell)](https://en.wikipedia.org/wiki/Comparison_of_SSH_clients).

### Mac OS X and Linux users

Mac OS X or Linux users may open a Terminal, and
type in the following command:

~~~
$ ssh username@login.palmetto.clemson.edu 
~~~

where `username` is your Clemson user ID.

### Windows

MobaXterm is the recommended SSH client for Windows and can be downloaded
[here](<http://mobaxterm.mobatek.net/download.html>).
This software is recommended because it is free and comes with
an comprehensive set of wrapper functions to support
components including:

1. an easy-to-use file transfer client
(which allows you to exchange files and folders
between your own computer and Palmetto)
2. an X11 server to support SSH tunneling of graphical programs 
(you no longer have to download, install, and run XMing separately).
3. a graphical port-forwarding interface to support easy access to 
web-based UI launched inside Palmetto

The user interface of MobaXterm contains a terminal, allowing Windows users to 
interact with Palmetto in a manner similar to Mac OS X and Linux users. 

After downloading and installing MobaXterm,
users can log-in using the connection information below:

~~~
$ ssh username@login.palmetto.clemson.edu 
~~~

where `username` is your Clemson user ID.

The Figure below illustrates how MobaXterm is used to connect to 
Palmetto. The frame containing files and directories to the left
is the content of your home directory. The red circle shows the 
two buttons that allow you to download from (light-green arrow) and 
upload to (purple arrow) Palmetto. The green circle indicates
a X11 server running inside MobaXterm. The orange circle shows the
button to call up portfowarding capabilities. 

<img src="{{site.baseurl}}/images/mobaxterm.png" style="width:1000px">

-----------------------------------------------------------------------

## Palmetto nodes

The Palmetto cluster is a collection of
several computers (nodes).
Each node has a [hostnames](https://en.wikipedia.org/wiki/Hostname)
used to identify that node,
such as `example.palmetto.clemson.edu`.

1. Login node: the login node is the entry-point to the cluster,
All users begin by logging-in to this node. The hostname of the login node
is `login.palmetto.clemson.edu`. It also has an "internal" hostname `login001`.

1. Compute nodes: the nodes that actually do most of the "computing" on the cluster.
Users make requests for temporary reservations
(or **jobs**) on the compute nodes for running their applications
(up to 72 or 168 hours, depending on
the hardware configuration of the node.
The compute nodes have hostnames `node0001.palmetto.clemson.edu`, `node0002.palmetto.clemson.edu`,
etc.,
The Palmetto cluster has over 2000 compute nodes.

1. Data transfer node: special node that is used for transfering
data to and from the cluster.
The data transfer node has the hostname `xfer01-ext.palmetto.clemson.edu`.

Palmetto is fairly "heterogeneous",
meaning that not all the compute nodes have the same hardware configuration.
Nodes differ in the number of CPU cores, amount of RAM, local disk space,
interconnect, etc.,
The hardware configuration of the different nodes is available in the file `/etc/hardware-table`:

~~~

$ cat /etc/hardware-table

PALMETTO HARDWARE TABLE      Last updated:  Dec 25 2016

PHASE COUNT  MAKE   MODEL    CHIP(0)                CORES  RAM(1)    /local_scratch   Interconnect         GPUs  PHIs SSD
 0      5    HP     DL580    Intel Xeon    7542       24   505 GB(2)    99 GB         1g, 10g, mx           0     0    0
 0      1    HP     DL980    Intel Xeon    7560       64     2 TB(2)    99 GB         1g, 10g, mx           0     0    0
 1    191    Dell   PE1950   Intel Xeon    E5345       8    12 GB       37 GB         1g, 10g, mx           0     0    0
 2    243    Dell   PE1950   Intel Xeon    E5410       8    12 GB       37 GB         1g, 10g, mx           0     0    0
 3    234    Sun    X2200    AMD   Opteron 2356        8    16 GB      193 GB         1g, 10g, mx           0     0    0
 4    329    IBM    DX340    Intel Xeon    E5410       8    16 GB      111 GB         1g, 10g, mx           0     0    0
 5a   370    Sun    X6250    Intel Xeon    L5420       8    32 GB       31 GB         1g, 10g, mx           0     0    0
 5b     9    Sun    X4150    Intel Xeon    E5410       8    16 GB       99 GB         1g, 10g, mx           0     0    0
 6     68    HP     DL165    AMD   Opteron 6176       24    48 GB      193 GB         1g, 10g, mx           0     0    0
 7a    42    HP     SL230    Intel Xeon    E5-2665    16    64 GB      240 GB         1g, 56g, fdr          0     0    0
 7b    12    HP     SL250s   Intel Xeon    E5-2665    16    64 GB      240 GB         1g, 56g, fdr          2(3)  0    0
 8a    71    HP     SL250s   Intel Xeon    E5-2665    16    64 GB      900 GB         1g, 56g, fdr          2(4)  0    300 GB(7)
 8b    57    HP     SL250s   Intel Xeon    E5-2665    16    64 GB      420 GB         1g, 56g, fdr          2(4)  0    0
 8c    68    Dell   PEC6220  Intel Xeon    E5-2665    16    64 GB      350 GB         1g, 10ge              0     0    0
 9     72    HP     SL250s   Intel Xeon    E5-2665    16   128 GB      420 GB         1g, 56g, fdr, 10ge    2(4)  0    0
10     80    HP     SL250s   Intel Xeon    E5-2670v2  20   128 GB      800 GB         1g, 56g, fdr, 10ge    2(4)  0    0
11a    40    HP     SL250s   Intel Xeon    E5-2670v2  20   128 GB      800 GB         1g, 56g, fdr, 10ge    2(6)  0    0
11b     4    HP     SL250s   Intel Xeon    E5-2670v2  20   128 GB      800 GB         1g, 56g, fdr, 10ge    0     2(8) 0
12     30    Lenovo NX360M5  Intel Xeon    E5-2680v3  24   128 GB      800 GB         1g, 56g, fdr, 10ge    2(6)  0    0
13     24    Dell   C4130    Intel Xeon    E5-2680v3  24   128 GB      800 GB         1g, 56g, fdr, 10ge    2(6)  0    0
14     12    HP     XL1X0R   Intel Xeon    E5-2680v3  24   128 GB      800 GB         1g, 56g, fdr, 10ge    2(6)  0    0
15     32    Dell   C4130    Intel Xeon    E5-2680v3  24   128 GB      800 GB         1g, 56g, fdr, 10ge    2(6)  0    0

TOTAL: 2021 nodes

PBS resource requests are always lowercase.

(0) CHIP has 3 resources:   chip_manufacturer, chip_model, chip_type
(1) Leave 2GB for the operating system when requesting memory in PBS jobs
(2) Specify queue "bigmem" to access the large memory machines
(3) 2 NVIDIA Tesla M2075 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2075"
(4) 2 NVIDIA Tesla K20m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k20"
(5) 2 NVIDIA Tesla M2070-Q cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2070q"
(6) 2 NVIDIA Tesla K40m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k40"
(7) Use resource request "ssd=true" to request a chunk with SSD in location /ssd1, /ssd2, and /ssd3 (100GB max each)
(8) Use resource request "nphis=[1|2]" to request phi nodes, the model is Xeon 7120p

~~~

When reading the `hardware-table` file,
remember that the cluster is composed of several phases (currently phases 0-15).
Each phase is composed of several nodes (e.g., phase 5a has 370 nodes)
with the same hardware configuration
(e.g., each node in phase 5a has 8 cores, 32 GB ram, 31 GB local disk space,
and 10 Gbps Myrinet interconnect.).

A useful command on the login node is `whatsfree`,
which gives information about how many nodes from each
phase are currently in use, free, or offlined for maintenance.

-----------------------------------------------------------------------

## Filesystems

Various filesystems are available for users to store data.
These differ in capacity, data-persistence, and efficiency,
and it is important that users understand which filesystem to use under which circumstances.

#### 1. `/home/` directory

The `/home` directory is shared and visible to all nodes.
Each user has a sub-directory in the `/home` directory (`/home/username`)
where they can store up to 100 GB.
Data stored here is backed-up on a nightly basis.

#### 2. `/scratch1`, `/scratch2`, `/scratch3` directories

The `/scratch` directories are shared and visible to all nodes.
Each user has a sub-directory in each of these scratch directories,
where they can store virtually unlimited amount of data **temporarily**.
Data stored in the scratch directories is not backed-up in any way,
and data that is unused for 30 days is automatically deleted (and cannot be recovered).

#### 3. The `local_scratch` filesystem

The `/home` and scratch systems are all
Networked Attached Storage (NAS) systems.
This means they are shared by all nodes.
In contrast to these NAS systems,
`local_scratch` is a filesystem local to each node.
Data in the `local_scratch` of a compute node can only be
accessed for the period of the job.

-----------------------------------------------------------------------

## Pre-installed software on Palmetto - modules

The Palmetto cluster provides a limited number of packages
(including site-licensed packages),
that can be used by all Palmetto users.
These packages are available as **modules**,
and must be activated/deactivated using the `module` command:

Command |   Purpose
--------|----------------------------------------------------------------------
`module avail` | List all packages available (on current system)
`module add package/version` | Add a package to your current shell environment
`module list` | List packages you have loaded
`module rm package/version` | Remove a currently loaded package
`module purge` | Remove *all* currently loaded packages

For example, to load the GCC (v4.8.1), CUDA Toolkit (v6.5.14)
and OpenMPI (v1.8.4) modules, you can use the command:

~~~
$ module add gcc/4.8.1 cuda-toolkit/6.5.14 openmpi/1.8.4
~~~

Then, check the version of `gcc`:

~~~
$ gcc --version
~~~

~~~
gcc (GCC) 4.8.1
Copyright (C) 2013 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
~~~

Some modules when loaded, implicitly load other modules as well.
If you use some modules to compile/install some software,
then you will probably have to load them
when running that software as well, otherwise you may see
errors about missing libraries/headers.
Modules do not remain loaded when you log out and log back in,
i.e., they are active only for the current session -
so you will need to load them for every session.

As an exercise,
examine the environment variables PATH, LIBRARY_PATH, etc.,
before and after loading some module:

~~~
$ echo $PATH
$ module add python/2.7.7
$ echo $PATH
~~~

You can also look at the modulefiles in `/software/modulefiles`
to get a better idea of how loading a module works.

-----------------------------------------------------------------------

## Job submission and control

To use the cluster,
users submit requests for temporary reservation
of hardware resources (CPU cores, memory, GPUs, etc.,).
These reservations are known as **jobs**,
and there are two kinds of jobs that you may request:

1.  **Interactive jobs** which allow you to interactively run
tasks using a command-line interface

2.  **Batch jobs** which allow you to schedule
the running of tasks

Interactive jobs are often best used during
prototyping, testing, and debugging;
but they are also used for running graphical applications
on the cluster.
Batch jobs are often best used for "production" runs, i.e., for
running a large number of tasks and/or long-running tasks
for which the user need not remain logged in to the cluster.

The Palmetto cluster uses the
Portable Batch Scheduling system (PBS)
to manage jobs.
The scheduler takes into account the resources (CPUs, memory, etc.,)
requested by the job.
If the resources are available, then the job is immediately
started.
If the resources are unavailable, the job is *queued*,
and will start whenever the requested resources are free.

### Interactive jobs

You can schedule a simple
interactive job using the `qsub` command
with the additional `-I` switch:

~~~
[username@login001 ~]$ qsub -I
~~~

Depending on whether there are resources available on
the cluster or not, the job may start immediately,
or may take a while.

When the job starts, you will see the following output:

~~~
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 4222909.pbs02 to start
qsub: job 4222909.pbs02 ready
[username@node0561 ~]$
~~~

We see that the prompt is changed from
`[username@login001 ~]$` to
something like `[username@node0561 ~]$`.
Here, `node0561` is the allocated compute node
(you may see a different node).
Once logged in to this node,
you can, for example,
set up your environment (i.e., load required modules)
and run some computationally intensive task.
In the below example commands,
we load the `anaconda3/2.5.0` module,
and run a Python script called `runsim.py`.

~~~
[username@node0561 ~]$ module add anaconda3/2.5.0
[username@node0561 ~]$ cd /home/username/projects/simulation
[username@node0561 simulation]$ python runsim.py
~~~

The `exit` command exits the current command shell
(and ends the interactive job), bringing you back to
the shell running on `login001`:

~~~
[username@node0561 ~]$ exit
logout

qsub: job 4222909.pbs02 completed
[username@login001 ~]$
~~~

The above job assumed the following default resource requests:

1.  1 node
2.  1 CPU core
3.  1 GB of memory
4.  30 minutes of wall time

Here is a more complex interactive job request:

~~~
$ qsub -I -l select=2:ncpus=2:mem=4gb,walltime=1:30:00
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 4222951.pbs02 to start
qsub: job 4222951.pbs02 ready

[atrikut@node1113 ~]$
~~~

In the above request, in addition to the `-I` switch,
we use the `-l` parameter to specify the job "limits".
Its value is `select=2:ncpus=2:mem=4gb,walltime=1:30:00`,
which specifies the limits as:

* 2 "chunks" of hardware, with 2 CPU cores per chunk
and 4 GB of memory per chunk
* 1 hour and 30 minutes of walltime

It is important to remember that job limits are
specified as a required number of "hardware chunks",
rather than "nodes".
Each of these "chunks" must be able to fit into
a node. For example, we cannot specify a "chunk" with
7000 CPU cores and 6 GPUs, as none of the compute nodes on
Palmetto can accommodate this chunk.

Several small chunks may be placed by the scheduler on
a single node, and indeed, at a given time,
a compute node may be shared by several users.
For example, in the above job limits specification
(2 chunks with 2 CPU cores and 4 GB of RAM each),
both chunks are small enough that they may be placed
on the same compute node.

When your job spans more than one node,
then the command-line session is started only on one
of these nodes known as the "lead node".
From the lead node, you can `ssh` into the other allocated
nodes (we will later see how to check what nodes your
jobs are running on).

**Note**: simply requesting a higher number of CPU cores
will generally not make your program run faster.

### Batch jobs

For batch jobs,
the job requirements and tasks to be run
must be specified in a *batch script*.
As an example, here is a simple batch script
(let's call it `example.pbs`):

~~~
#PBS -N example
#PBS -l select=1:ncpus=1:mem=2gb,walltime=00:10:00

module add gcc/4.8.1

cd /home/username
echo Hello World from `hostname`
sleep 60
~~~

Lines beginning with `#PBS` are directives to the scheduler,
and ignored by the compute nodes.
The other lines are executed on the allocated compute nodes
(actually, they are executed only by the "lead" node),
and ignored by the scheduler.

To submit a batch job, you can use `qsub` followed
by the name of the script:

~~~
$ qsub example.pbs

4256957.pbs02
~~~

The output of `qsub` is a unique
job ID which can be used to query the status of a job
using the `qstat` command:

~~~
$ qstat 4256957.pbs02


Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
4256957.pbs02     example          atrikut           00:00:00 R c1_solo
~~~

The output of `qstat` shows,
among other things, the status of the job
(under the column `S`). Here, the status is shown as Running (`R`).
Jobs may also have a Queued (`Q`) status, or Error (`E`) status.
Queued jobs are waiting for
the requested resources to become available
before they can be run.

Once the job is complete,
you should see the following output for `qstat`:

~~~
$ qstat 4256957.pbs02
qstat: 4256957.pbs02 Job has finished, use -x or -H to obtain historical job information
~~~

You should also see two new files
in the working directory of your job
with names `<jobname>.o<job ID>` and `<jobname>.e<job ID>`:

~~~
$ ls

example.o4256957    example.e4256957
~~~

The `.o` file contains any standard output your job may produce
and the `.e` file contains any standard errors.
In addition, if your job produces any data,
such as logs, figures, etc., these will appear in the working directory
(or whatever directory you configured them to be placed in).

~~~
$ cat example.o4256957

Hello World from node0570
RESOURCES REQUESTED
mem=2gb,ncpus=1,walltime=00:10:00

RESOURCES USED
cpupercent=0,cput=00:00:00,mem=0kb,ncpus=1,vmem=0kb,walltime=00:00:12
~~~

The above script included two directives for the scheduler:

~~~
#PBS -N example
#PBS -l select=1:ncpus=1:mem=2gb,walltime=00:10:00
~~~

These directives specified the name for the job
(using the `-N` parameter),
and a list of resources required (using the `-l` parameter).
Following is a list of parameters you can use
(note that these parameters can also be used in interactive job requests):

Parameter | Purpose | Example
--------- | ------- | -------- 
`-N` | Job name (7 characters)	| `-N maxrun1`
`-l` | Job limits (lowercase L), hardware & other requirements for job | `-l select=1:ncpus=8:mem=1gb`
`-q` | Queue to direct this job to (`workq` is the default, `supabad` is an example of specific research group's job queue) | `-q supabad`
`-o` | Path to stdout file for this job (environment variables are no longer accepted here) | `-o stdout.txt`
`-e` | Path to stderr file for this job (environment variables are no longer accepted here) | `-e stderr.txt`
`-M` | E-mail for messages from the PBS server	 | `-M username@clemson.edu`
`-m` | Type of notification you wish to receive (b,e,a,n) | `-m ea`

### Querying job information and deleting jobs

As mentioned above, the `qstat` command can be used
to query the status of a particular job:

~~~
$ qstat 7600424.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
7600424.pbs02     pi-mpi           atrikut           00:00:00 R c1_single
~~~

To list the job IDs and status of all your jobs,
you can use the `-u` switch:

~~~
$ qstat -u username


pbs02:
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
7600567.pbs02   username  c1_singl pi-mpi-1     1382   4   8    4gb 00:05 R 00:00
7600569.pbs02   username  c1_singl pi-mpi-2    20258   4   8    4gb 00:05 R 00:00
7600570.pbs02   username  c1_singl pi-mpi-3     2457   4   8    4gb 00:05 R 00:00
~~~

Once a job has finished running,
`qstat -xf` can be used to obtain detailed job information:

~~~
$ qstat -xf 7600424.pbs02

Job Id: 7600424.pbs02
    Job_Name = pi-mpi
    Job_Owner = atrikut@login001.palmetto.clemson.edu
    resources_used.cpupercent = 103
    resources_used.cput = 00:00:04
    resources_used.mem = 45460kb
    resources_used.ncpus = 8
    resources_used.vmem = 785708kb
    resources_used.walltime = 00:02:08
    job_state = F
    queue = c1_single
    server = pbs02
    Checkpoint = u
    ctime = Tue Dec 13 14:09:32 2016
    Error_Path = login001.palmetto.clemson.edu:/home/atrikut/MPI/pi-mpi.e7600424

    exec_host = node0088/1*2+node0094/1*2+node0094/2*2+node0085/0*2
    exec_vnode = (node0088:ncpus=2:mem=1048576kb:ngpus=0:nphis=0)+(node0094:ncp
	us=2:mem=1048576kb:ngpus=0:nphis=0)+(node0094:ncpus=2:mem=1048576kb:ngp
	us=0:nphis=0)+(node0085:ncpus=2:mem=1048576kb:ngpus=0:nphis=0)
    Hold_Types = n
    Join_Path = oe
    Keep_Files = n
    Mail_Points = a
    Mail_Users = atrikut@clemson.edu
    mtime = Tue Dec 13 14:11:42 2016
    Output_Path = login001.palmetto.clemson.edu:/home/atrikut/MPI/pi-mpi.o760042
	4
    Priority = 0
    qtime = Tue Dec 13 14:09:32 2016
    Rerunable = True
    Resource_List.mem = 4gb
    Resource_List.mpiprocs = 8
    Resource_List.ncpus = 8
    Resource_List.ngpus = 0
    Resource_List.nodect = 4
    Resource_List.nphis = 0
    Resource_List.place = free:shared
    Resource_List.qcat = c1_workq_qcat
    Resource_List.select = 4:ncpus=2:mem=1gb:interconnect=1g:mpiprocs=2
    Resource_List.walltime = 00:05:00
    stime = Tue Dec 13 14:09:33 2016
    session_id = 2708
    jobdir = /home/atrikut
    substate = 92
    Variable_List = PBS_O_SYSTEM=Linux,PBS_O_SHELL=/bin/bash,
	PBS_O_HOME=/home/atrikut,PBS_O_LOGNAME=atrikut,
	PBS_O_WORKDIR=/home/atrikut/MPI,PBS_O_LANG=en_US.UTF-8,
	PBS_O_PATH=/software/examples/:/home/atrikut/local/bin:/usr/lib64/qt-3
	.3/bin:/opt/pbs/default/bin:/opt/gold/bin:/usr/local/bin:/bin:/usr/bin:
	/usr/local/sbin:/usr/sbin:/sbin:/opt/mx/bin:/home/atrikut/bin,
	PBS_O_MAIL=/var/spool/mail/atrikut,PBS_O_QUEUE=c1_workq,
	PBS_O_HOST=login001.palmetto.clemson.edu
    comment = Job run at Tue Dec 13 at 14:09 on (node0088:ncpus=2:mem=1048576kb
	:ngpus=0:nphis=0)+(node0094:ncpus=2:mem=1048576kb:ngpus=0:nphis=0)+(nod
	e0094:ncpus=2:mem=1048576kb:ngpus=0:nphis=0)+(node0085:ncpus=2:mem=1048
	576kb:ngpus=0:nphis=0) and finished
    etime = Tue Dec 13 14:09:32 2016
    run_count = 1
    Stageout_status = 1
    Exit_status = 0
    Submit_arguments = job.sh
    history_timestamp = 1481656302
    project = _pbs_project_default
~~~

Similarly, to get detailed information about
a running job, you can use `qstat -f`.

To delete a job (whether in queued, running or error status),
you can use the `qdel` command.

~~~
$ qdel 7600424.pbs02
~~~

### Maximum walltime of jobs

The maximum wall time for all jobs in the default `workq`
queue is 168 hours (1 week) for jobs running on
Phases 1-6 of the cluster, and 72 hours (3 days)
for jobs running on all other phases.

Any jobs that run for longer than the specified wall time
will be abruptly stopped. You should specify a wall time
that is slightly longer than what is actually required
for your job to allow for delays in network communication
or I/O.

### Execution queues and job limits

When submitting jobs, you can specify a "queue"
to submit to using the `-q` switch; for example:

~~~
$ qsub example.pbs -q bigmem
~~~

If not specified, the default queue for all users
is the `workq` queue.
The only other general queue is the `bigmem` queue,
which you must submit to if you require any
of the large-memory machines from Phase 0 of the cluster.

Jobs that are submitted to the default `workq` queue
are forwarded to 
specific *job execution queues* based on job critera
(e.g., how many cores are required, how much memory, etc.,).

Each execution queue has its own limits for
how many jobs can be running at one time,
and how many jobs can 
be waiting in that execution queue.
These limits change based on availability and usage of the
cluster.

You can see what the current limits
are using the `checkqueuecfg` command:

~~~
$ checkqueuecfg


Jobs submitted to WORKQ or BIGMEM will end up in one of the
general execution
queues listed below based on the number of cores or gpus
or phis requested.

The maximum number of running jobs per user in execution queues may vary
throughout the day depending on cluster load.  Jobs in owner queues may
preempt jobs in general execution queues and the preempted jobs will be
automatically requeued.

NOTE: Each core comes with 2gb ram, so if a job is asking for 1 core and
and 16gb memory, the job will run in the solo execution queue.  This is
equivalent to 8 cores worth of memory therefore this one
job counts as 8 jobs towards the user's maximum for the queue.

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

GPU QUEUES     min_gpus   max_gpus   max_jobs
gpu_small             1          4          5
gpu_medium            5         16          3
gpu_large            17        128          1

PHI QUEUE      min_phis   max_phis   max_jobs
phi_small             1          2          1

BIGMEM QUEUE   min_cpus   max_cpus   max_jobs
bigmem_e              1         64          1
~~~

If you are part of an "owner" group on the cluster,
then your group may have additional submission queues that
you can submit to. Jobs submitted to these owner queues
have priortiy over general `workq` jobs and can pre-empt such
jobs.

#### Job limits specification examples

There are various resources you can specify in your
job limits specification (`-l`). These apply to
interactive jobs as well as batch jobs.
Here are some examples for the job limits specifications:

~~~
-l select=1:ncpus=8:chip_manufacturer=intel:interconnect=10g
-l select=1:ncpus=8:chip_model=opteron:interconnect=10g
-l select=1:ncpus=16:chip_type=e5-2665:interconnect=56g:mem=62gb,walltime=16:00:00
-l select=1:ncpus=8:chip_type=2356:interconnect=10g:mem=15gb
-l select=1:ncpus=1:node_manufacturer=ibm:mem=15gb,walltime=00:20:00
-l select=1:ncpus=4:mem=15gb:ngpus=2,walltime=00:20:00
-l select=1:ncpus=4:mem=15gb:ngpus=1:gpu_model=k40,walltime=00:20:00
-l select=1:ncpus=2:mem=15gb:host=node1479,walltime=00:20:00
-l select=2:ncpus=2:mem=15gb,walltime=00:20:00,place=scatter # force each chunk to be on a different node
-l select=2:ncpus=2:mem=15gb,walltime=00:20:00,place=pack # force each chunk to be on the same node
~~~

and examples of options you can use in the job limit specification:

~~~
chip_manufacturer=amd
chip_manufacturer=intel
chip_model=opteron
chip_model=xeon
chip_type=e5345
chip_type=e5410
chip_type=l5420
chip_type=x7542
chip_type=2356
chip_type=6172
chip_type=e5-2665
node_manufacturer=dell
node_manufacturer=hp
node_manufacturer=ibm
node_manufacturer=sun
gpu_model=k20
gpu_model=k40
interconnect=1g     (1 Gbps Ethernet)
interconnect=10g    (10 Gbps Myrinet, same as mx)
interconnect=10ge   (10 Gbps Ethernet)
interconnect=56g    (56 Gbps FDR InfiniBand, same as fdr)
interconnect=mx     (10 Gbps Myrinet, same as 10g)
interconnect=fdr    (56 Gbps FDR InfiniBand, same as 56g)
ssd=true   			(Use a node with an SSD hard drive)
~~~

Of course, not all the above options are all
compatible with each other; you should review the file
`/etc/hardware-table` when drafting job limits specifications.

### Example PBS scripts

A list of example PBS scripts for submitting
jobs to the Palmetto cluster can be found
[here](https://github.com/clemsonciti/palmetto-examples).

### Summary of commands for job submission and job control

Command                         | Action
--------------------------------|--------------------------------
`qsub xyz.pbs`                  | Submit the job script `xyz.pbs`
`qstat <job id>`                | Check the status of the job with given job ID
`qstat -u <username>`           | Check the status of all jobs submitted by given username
`qstat -xf <job id>`            | Check detailed information for job with given job ID
`qsub -q <queuename> xyz.pbs`   | Submit to queue `queuename`
`qdel <job id>`                 | Delete the job (queued or running) with given job ID
`qdel -Wforce <job id>`         | Use when job not responding to just `qdel`

For more details and more advanced commands for submitting and controlling jobs,
please refer to the [PBS Professional User's Guide](http://www.pbsworks.com/pdfs/PBSUserGuide14.2.pdf).

-----------------------------------------------------------------------

## Data Management

### Transfering data in and out of the cluster

#### Small files

To transfer data in and out of the cluster,
you can use an SFTP client
such as FileZilla (https://filezilla-project.org/).
When connecting to Palmetto for File transfer,
always remember to connect to the special
data transfer node, and **not** the login node.

Field                   | Value
----------------------- | ----------------------------
Host Name               | `xfer01-ext.palmetto.clemson.edu`
User Name               | `username`
Port Number             | `22`

On Unix systems, you can also
use the `scp` (secure copy) command to perform file transfers.
For example,
here is the `scp` command to copy a file from the
current directory on your local machine to your
`/home/username` directory on Palmetto
(this command is entered into a terminal running on your local machine):

~~~
$ scp myfile.gvrx username@xfer01-ext.palmetto.clemson.edu:/home/username
~~~

... and to do the same in reverse,
i.e., copy from Palmetto to your local machine.
(again, from a terminal running on your local machine):

~~~
$ scp username@xfer01-ext.palmetto.clemson.edu:/home/username/myfile.gvrx .
~~~

The `.` represents the working directory on the local machine.

#### Transfering larger files

For larger files, we recommend
using the [Globus](https://www.globus.org/)
file transfer application.
Here, we demonstrate how to use Globus Online
to transfer files between Palmetto and a local machine (laptop).
However, Globus can be used for file transfers to/from other locations as well.

1.  You will need to have a Globus account set up to begin.
    Visit [https://www.globus.org/](https://www.globus.org/)
    and set up a Globus account.

2.  To begin transfering files,
    navigate to the Globus Online transfer utility
    here:
    [https://www.globus.org/app/transfer](https://www.globus.org/app/transfer).

3.  The transfer utility allows you to transfer
    files between "endpoints".
    You will need to set your local machine
    as a Globus Connect Personal Endpoint for the file transfer.
    As a part of this step, you must install the
    Globus Connect Personal application
    (see here:
    [https://www.globus.org/app/endpoints/create-gcp](https://www.globus.org/app/endpoints/create-gcp)
    ).
    After installing,
    ensure that the application is running.
    You should then be able to set your local machine as one endpoint.
    In the figure below, the endpoint is named `My Personal Mac`.

4.  As the second endpoint,
    choose `clemson#xfer01-ext.palmetto.clemson.edu`.

5.  You can now transfer files between any locations on your
    local machine and the Palmetto cluster.

<img src="{{site.baseurl}}/images/globus.png" style="width:750px">

### Storing data in the long term

While the scratch systems can be used
to keep data during analysis/processing temporarily,
they are **not** meant for long-term storage of data.
The home directory is backed-up and safer
for storing data in the long term.
However, it is limited to only 100 GB.

If you need to store and access larger amounts of
data on the Palmetto cluster for an extended
period of time, the following options are available:

#### 1. Temporary reservations

All users may apply for temporary reservation
of the following resources:

1. Up to 150 TB of long-term storage
2. Up to 8.5 TB of fast SSD scratch space

All requests will be reviewed by Clemson University
Computational Advisory Team (CU-CAT).
Reservation requests can be made [here](https://citi.sites.clemson.edu/new-reservation/
).

#### 2. Purchased storage

Users or groups may also
purchase storage in 1 TB increments.
For details about purchased storage, please contact the Palmetto support staff
(<ithelp@clemson.edu>, and include the word "Palmetto" in the subject line).
