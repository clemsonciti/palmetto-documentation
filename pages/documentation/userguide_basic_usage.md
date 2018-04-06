---
title: Basic Usage
keywords: [login,submit,job,data,transfer,PBS,script]
sidebar: documentation_sidebar
permalink: userguide_basic_usage.html
---

## Logging in

Any user with a Palmetto Cluster account can log-in using
[SSH (Secure Shell)](https://en.wikipedia.org/wiki/Comparison_of_SSH_clients).
Mac OS X and Linux systems come with an SSH client installed,
while Windows users will need to download one.

All connections to Palmetto require two-factor authentication (2FA).
If you are not enrolled in 2FA yet,
you may enroll using the link <https://2fa.clemson.edu/>.

### Mac OS X and Linux users

Mac OS X or Linux users may open a Terminal, and
type in the following command:

~~~
$ ssh username@login.palmetto.clemson.edu 
~~~

where `username` is your Clemson user ID.
You will be prompted for both your password and DUO authentication.

### Windows

MobaXterm is the recommended SSH client for Windows and can be downloaded
[here](<http://mobaxterm.mobatek.net/download.html>).
This software is recommended because it is free and comes with:

* A built-in file transfer client,
which allows you to exchange files and folders
between your own computer and Palmetto

* An X11 server which allows you to run graphical
programs on Palmetto cluster

* A graphical port-forwarding interface
to support easy access to web-based programs launched inside Palmetto

After downloading and installing MobaXterm,
users can log-in by following these steps:

1.  Launch the MobaXterm program

    <img src="{{site.baseurl}}/images/mobaxterm_01.png" style="width:1000px">

2.  On the top-left corner of MobaXterm, click the **Session** button. Select
    the SSH setting and confirm that the following settings are set:

    Parameter           |   Value
    --------------------|-------------------------------------
    Remote host         | `login.palmetto.clemson.edu`
    Port                |  22
    X11-Forwarding      | enabled
    Compression         | enabled
    Remote environment  | Interactive shell
    SSH-browser type    | **SCP (enhanced speed)**

    <img src="{{site.baseurl}}/images/mobaxterm_02.png" style="width:1000px">

3.  Click **OK** and a new session window will be opened, where you will be
    prompted for your Palmetto password and the DUO authentication.

    <img src="{{site.baseurl}}/images/mobaxterm_03.png" style="width:1000px">

4.  After being authenticated, you will login to the **login001** node.

    <img src="{{site.baseurl}}/images/mobaxterm_04.png" style="width:1000px">

    All settings for this session are saved, and for future logins, you
    can select this session from the **Recent sessions** form of the main MobaXterm
    window as well as the **Saved sessions** tab of the side window. The side
    window can be displayed or hidden by clicking on the blue double-arrow sign
    on the top left of MobaXterm.

    <img src="{{site.baseurl}}/images/mobaxterm_05.png" style="width:1000px">

    <img src="{{site.baseurl}}/images/mobaxterm_06.png" style="width:1000px">

    MobaXterm also comes with a built-in file browser and transfer GUI
    (SSH-browser). This GUI is accessible via the **SCP** tab of the side
    window. Using the Upload (green arrow pointing up) and
    and Dowload (blue arrow pointing down) buttons at the top of the SCP tab,
    you can easily transfer files between Palmetto and your local computer.

    <img src="{{site.baseurl}}/images/mobaxterm_07.png" style="width:1000px">

## Basic tasks

### Storing files and folders

#### Home and scratch directories

Various filesystems are available for users to store data.
These differ in capacity, data-persistence, and efficiency,
and it is important that users understand which filesystem
to use under which circumstances.

Location                |	Available space                     | Notes
------------------------|---------------------------------------|---------------------------------------------------------------------------
`/home/username`        |   100 GB per user                     | Backed-up nightly, permanent storage space accessible from all nodes
`/scratch1/username`    |   233 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, OrangeFS Parallel File Sytem
`/scratch2/username`    |   160 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, XFS
`/scratch3/username`    |   129 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, ZFS
`/local_scratch`        |   Varies between nodes (99GB-800GB)   | Per-node temporary work space, accessible only for the lifetime of job

The `/home` and `/scratch` directories are shared by all nodes.
In contrast, each node has its own `/local_scratch` directory.

All data in the `/home` directory is permanent
(not automatically deleted) and backed-up on a nightly basis.
If you lose data in the `/home` directory,
it may be possible to recover it if it was previously backed up.

Data in the `/scratch` directories is **not** backed up,
and any data that is untouched for 30 days is automatically
removed from the `/scratch` directories.
Data that cannot easily be reproduced should **not** be stored
in the `/scratch` directories,
and any data that is not required should be
removed as soon as possible.

See [this guide]({{site.baseurl}}/userguide_howto_choose_right_filesystem.html)
on how to choose the appropriate filesystem for your work.

#### Temporary reservations

All users may apply for temporary reservation
of the following resources:

1. Up to 150 TB of long-term storage
2. Up to 8.5 TB of fast SSD scratch space

All requests will be reviewed by Clemson University
Computational Advisory Team (CU-CAT).
Reservation requests can be made [here](https://citi.sites.clemson.edu/new-reservation/
).

#### Purchased storage

Users or groups may also
purchase storage in 1 TB increments.
For details about purchased storage, please contact the Palmetto support staff
(<ithelp@clemson.edu>, and include the word "Palmetto" in the subject line).

### Moving data in and out of the cluster

#### Small files (kilobytes or a few megabytes)

On Windows machines, using the MobaXterm SSH client,
the built-in file browser can be used (**SCP** tab of the side window).
Using the Upload (green arrow pointing up) and
and Dowload (blue arrow pointing down) buttons at the top of the SCP tab,
you can easily transfer small files between Palmetto and your local computer.

<img src="{{site.baseurl}}/images/mobaxterm_07.png" style="width:1000px">

On Unix systems, you can also use the `scp` (secure copy) command to
perform file transfers. The general form of the `scp` command is:

~~~
$ scp <path_to_source> username@xfer01-ext.palmetto.clemson.edu:<path_to_destination>
~~~

For example, here is the `scp` command to copy a file from the
current directory on your local machine to your
`/home/username` directory on Palmetto
(this command is entered into a terminal running on your local machine,
**not** on Palmetto):

~~~
$ scp myfile.txt username@xfer01-ext.palmetto.clemson.edu:/home/username
~~~

... and to do the same in reverse,
i.e., copy from Palmetto to your local machine.
(again, from a terminal running on your local machine,
**not** on Palmetto):

~~~
$ scp username@xfer01-ext.palmetto.clemson.edu:/home/username/myfile.txt .
~~~

The `.` represents the working directory on the local machine.

For folders, include the `-r` switch:

~~~
$ scp -r myfolder username@xfer01-ext.palmetto.clemson.edu:/home/username
~~~

#### Transfering larger files (more than a few megabytes)

For larger files, we recommend using the [Globus](https://www.globus.org/)
file transfer application. Here, we demonstrate how to use Globus Online
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

### Checking available compute hardware

The login node `login001` (the node that users first log-in to),
is not meant for running computationally intensive tasks.
Instead, users must reserve hardware from the **compute nodes**
of the cluster. Currently, Palmetto cluster has over 2020 compute
nodes. The hardware configuration of the different nodes is available
in the file `/etc/hardware-table`:

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

The compute nodes are divided into "phases" (currently phases 0-15).
Each phase is composed of several nodes with identical configuration,
e.g., each node in phase 5a has 8 cores, 32 GB ram, 31 GB local disk space,
and 10 Gbps Myrinet interconnect.

A useful command on the login node is `whatsfree`, which gives information
about how many nodes from each phase are currently in use, free, or offlined
for maintenance.

Later sections of this guide will describe how to submit **jobs** to
the cluster, i.e., reserve compute nodes for running computational tasks.

### Checking and using available software

The Palmetto cluster provides a limited number of packages
(including site-licensed packages),
that can be used by all Palmetto users.
These packages are available as **modules**,
and must be activated/deactivated using the `module` command:

Command                         |   Purpose
--------------------------------|----------------------------------------------------------------------
`module avail`                  | List all packages available (on current system)
`module add package/version`    | Add a package to your current shell environment
`module list`                   | List packages you have loaded
`module rm package/version`     | Remove a currently loaded package
`module purge`                  | Remove *all* currently loaded packages

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
to understand what happens when you add a module.

### Start an interactive job

An interactive job can be started using the `qsub` command.
Here is an example of an interactive job:

~~~
[username@login001 ~]$ qsub -I -l select=1:ncpus=2:mem=4gb,walltime=4:00:00
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 8730.pbs02 to start
qsub: job 8730.pbs02 ready

[username@node0021 ~]$ module add python/3.4
[username@node0021 ~]$ python runsim.py
.
.
.
[username@node0021 ~]$ exit
[username@login001 ~]$
~~~

Above, we request an interactive job using 1 "chunk" of hardware (`select=1`),
2 CPU cores per "chunk", and 4gb of RAM per "chunk", for a wall time of 4 hours.
Once these resources are available, we receive a Job ID (`8730.pbs02`),
and a command-line session running on `node0021`.

### Submit a batch job

Interactive jobs require you to be logged-in while your tasks are running.
In contrast, you may logout after submitting a batch job,
and examine the results at a later time. This is useful when you need to
run several computational tasks to the cluster, and/or when your
computational tasks are expected to run for a long time.

To submit a batch job, you must prepare a **batch script**
(you can do this using an editor like `vim` or `nano`).
Following is an example of a batch script (call it `example.pbs`).
In the batch job below, we really don't do anything useful
(just sleep or "do nothing" for 60 seconds),

~~~
#PBS -N example
#PBS -l select=1:ncpus=1:mem=2gb,walltime=00:10:00

module add gcc/4.8.1

cd /home/username
echo Hello World from `hostname`
sleep 60
~~~

After saving the above file, you can submit the batch job using `qsub`:

~~~
[username@login001 ~]$ qsub example.pbs
8738.pbs02
~~~

The returned job ID can be used to query the status of the job (using `qstat`)
(or delete it using `qdel`):

~~~
[username@login001 ~]$ qstat 8738.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
8738.pbs02        example          username           00:00:00 R c1_solo
~~~

Once the job is completed, you will see the files `example.o8738` (containing output if any)
and `example.e8738` (containing errors if any) from your job.

~~~
[username@login001 ~]$ cat example.o8738
Hello World from node0230.palmetto.clemson.edu
~~~

## Job submission and control on Palmetto

The Palmetto cluster uses the Portable Batch Scheduling system (PBS)
to manage jobs. Here are some basic PBS commands
for submitting, querying and deleting jobs:

Command                         | Action
--------------------------------|--------------------------------
`qsub -I`                       | Submit an interactive job (reserves 1 core, 1gb RAM, 30 minutes walltime)
`qsub xyz.pbs`                  | Submit the job script `xyz.pbs`
`qstat <job id>`                | Check the status of the job with given job ID
`qstat -u <username>`           | Check the status of all jobs submitted by given username
`qstat -xf <job id>`            | Check detailed information for job with given job ID
`qsub -q <queuename> xyz.pbs`   | Submit to queue `queuename`
`qdel <job id>`                 | Delete the job (queued or running) with given job ID
`qpeek <job id>`                | "Peek" at the standard output from a running job
`qdel -Wforce <job id>`         | Use when job not responding to just `qdel`

For more details and more advanced commands for submitting and controlling jobs,
please refer to the [PBS Professional User's Guide](http://www.pbsworks.com/pdfs/PBSUserGuide14.2.pdf).

### PBS job options

The following switches can be used either
with `qsub` on the command line,
or with a `#PBS` directive in a batch script.

Parameter | Purpose | Example
----------|---------|---------
`-N`      | Job name (7 characters)	| `-N maxrun1`
`-l`      | Job limits (lowercase L), hardware & other requirements for job. | `-l select=1:ncpus=8:mem=1gb`
`-q`      | Queue to direct this job to (`workq` is the default, `supabad` is an example of specific research group's job queue) | `-q supabad`
`-o`      | Path to stdout file for this job (environment variables are not accepted here) | `-o stdout.txt`
`-e`      | Path to stderr file for this job (environment variables are not accepted here) | `-e stderr.txt`
`-M`      | E-mail for messages from the PBS server	 | `-M username@clemson.edu`
`-j oe`   | Join the output and error streams and write to a single file | `-j oe`

For example, in a batch script:

~~~
#PBS -N hydrogen
#PBS -l select=1:ncpus=24:mem=200gb,walltime=4:00:00
#PBS -q bigmem
#PBS -j oe
~~~

And in an interactive job request on the command line:

~~~
$ qsub -I -N hydrogen -q bigmem -j oe -l select=1:ncpus=24:mem=200gb,walltime=4:00:00
~~~

### Resource limits specification

The `-l` switch provided to `qsub` or along with the `#PBS` directive
can be used to specify the amount and kind of compute hardware (cores, memory, GPUs, interconnect, etc.,),
its location, i.e., the node(s) and phase from which to request hardware,

Option      | Purpose
------------|--------------------------------------------------------------
`select`    | Number of chunks and resources per chunk. Two or more "chunks" can be placed on a single node, but a single "chunk" cannot span more than one node.
`walltime`  | Expected wall time of job (job is terminated after this time)
`place`     | Controls the placement of the different chunks

Here are some examples of resource limits specification:

~~~
-l select=1:ncpus=8:chip_model=opteron:interconnect=10g
-l select=1:ncpus=16:chip_type=e5-2665:interconnect=56g:mem=62gb,walltime=16:00:00
-l select=1:ncpus=8:chip_type=2356:interconnect=10g:mem=15gb
-l select=1:ncpus=1:node_manufacturer=ibm:mem=15gb,walltime=00:20:00
-l select=1:ncpus=4:mem=15gb:ngpus=2,walltime=00:20:00
-l select=1:ncpus=4:mem=15gb:ngpus=1:gpu_model=k40,walltime=00:20:00
-l select=1:ncpus=2:mem=15gb:host=node1479,walltime=00:20:00
-l select=2:ncpus=2:mem=15gb,walltime=00:20:00,place=scatter    # force each chunk to be on a different node
-l select=2:ncpus=2:mem=15gb,walltime=00:20:00,place=pack       # force each chunk to be on the same node
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

### Querying job information and deleting jobs

The `qstat` command can be used
to query the status of a particular job:

~~~
$ qstat 7600424.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
7600424.pbs02     pi-mpi           username           00:00:00 R c1_single
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
    Job_Owner = username@login001.palmetto.clemson.edu
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
    Error_Path = login001.palmetto.clemson.edu:/home/username/MPI/pi-mpi.e7600424

    exec_host = node0088/1*2+node0094/1*2+node0094/2*2+node0085/0*2
    exec_vnode = (node0088:ncpus=2:mem=1048576kb:ngpus=0:nphis=0)+(node0094:ncp
	us=2:mem=1048576kb:ngpus=0:nphis=0)+(node0094:ncpus=2:mem=1048576kb:ngp
	us=0:nphis=0)+(node0085:ncpus=2:mem=1048576kb:ngpus=0:nphis=0)
    Hold_Types = n
    Join_Path = oe
    Keep_Files = n
    Mail_Points = a
    Mail_Users = username@clemson.edu
    mtime = Tue Dec 13 14:11:42 2016
    Output_Path = login001.palmetto.clemson.edu:/home/username/MPI/pi-mpi.o760042
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
    jobdir = /home/username
    substate = 92
    Variable_List = PBS_O_SYSTEM=Linux,PBS_O_SHELL=/bin/bash,
	PBS_O_HOME=/home/username,PBS_O_LOGNAME=username,
	PBS_O_WORKDIR=/home/username/MPI,PBS_O_LANG=en_US.UTF-8,
	PBS_O_PATH=/software/examples/:/home/username/local/bin:/usr/lib64/qt-3
	.3/bin:/opt/pbs/default/bin:/opt/gold/bin:/usr/local/bin:/bin:/usr/bin:
	/usr/local/sbin:/usr/sbin:/sbin:/opt/mx/bin:/home/username/bin,
	PBS_O_MAIL=/var/spool/mail/username,PBS_O_QUEUE=c1_workq,
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

### Job limits on Palmetto

#### Walltime

Jobs running in phases 1-6 of the cluster (nodes with interconnect `mx`)
can run for a maximum walltime of 168 hours (7 days).

Job running in phases 7 and higher of the cluster
can run for a maximum walltime of 72 hours (3 days).

#### Number of jobs

When you submit a job,
it is forwarded to a specific **execution queue**
based on job critera
(how many cores, RAM, etc.).
There are three classes of execution queues:

1. MX queues (`c1_` queues): jobs submitted to run on the older hardware (phases 1-6)
will be forwarded to theses queues.

2. IB queues (`c2_` queues): jobs submitted to run the newer hardware (phases 6 and up)
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
the `c2_single` queue:

~~~
[username@login001 ~]$ qstat 9567792.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
9567792.pbs02     STDIN            username                  0 Q c2_single
~~~

From the output of `checkqueuecfg` above,
we see that each user can have a maximum of **5** running jobs in this queue.

### Example PBS scripts

A list of example PBS scripts for submitting
jobs to the Palmetto cluster can be found
[here](https://github.com/clemsonciti/palmetto-examples).

## Common problems/issues

### Program crashes on login node with message `Killed`

When running commands or editing files on the login node, users may
notice that their processes end abruptly with the error message `Killed`.
Processes with names such as `a.out`, `matlab`, etc.,
are automatically killed on the login node because they may consume
excessive computational resources. Unfortunately, this also means that
benign processes, such as editing a file with the word `matlab` as part
of its name could also be killed.

**Solution:** Request an interactive session on a compute node (`qsub -I`),
and then run the application/command.

### Home or scratch directories are sluggish or unresponsive

The `/home` and `/scratch` directories can become slow/unresponsive
when a user (or several users) read/write large amounts of data to
these directories. When this happens, all users are affected as these
filesystems are shared by all nodes of the cluster.

To avoid this issue, keep in mind the following:

1.  **Never** use the `/home` directory as the working directory for
jobs that read/write data. If too many jobs read/write data to the `/home`
directory, it can render the cluster unusable by all users.
Copy any input data to one of the `/scratch` directories and use
that `/scratch` directory as the working directory for jobs.
Periodically move important data back to the `/home` directory.

1.  Try to use `/local_scratch` whenever possible. Unlike `/home`
or the `/scratch` directories, which are shared by all nodes, each
node has its own `/local_scratch` directory. It is much faster to read/write
data to `/local_scratch`, and doing so will not affect other users.
(see example [here](https://www.palmetto.clemson.edu/palmetto/userguide_howto_choose_right_filesystem.html).

