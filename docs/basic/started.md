# Starting out on Palmetto

When you first logged into Palmetto, you will be presented with a welcome message. This is called the **motd**, or **Message of the day**. This message typically contains a quick introduction to Palmetto, links to user guides, and a list of useful commands.
Information about upcoming maintenance will also be posted here.

<img src="../../images/basic/started/palmetto_01.png" style="width:600px">

## Available storage on Palmetto

Various filesystems are available for users to store data.
These differ in capacity, data-persistence, and efficiency,
and it is important that users understand which filesystem
to use under which circumstances.

#### Home and scratch directories

| Location             | Available space                   | Notes                                                                                      |
| -------------------- | --------------------------------- | ------------------------------------------------------------------------------------------ |
| `/home/username`     | 100 GB per user                   | Backed-up nightly, permanent storage space accessible from all nodes                       |
| `/scratch1/username` | 1.9 PB shared by all users        | Not backed up, temporary work space accessible from all nodes, BeeGFS Parallel File System |
| `/local_scratch`     | Varies between nodes (99GB-3.6TB) | Per-node temporary work space, accessible only for the lifetime of job                     |

- The `/home` and `scratch1` directories are shared by all nodes.
- Each node has its own `/local_scratch` directory.
- The parallel file system, `/scratch1`, is best suited for workflows issuing large read or write requests or creating a large number of files and
  directories. A large read or write is when data is accessed in large chunks, such as 1MB at a time. A large number of files and directories refers
  to workflows that generate thousands of files and directories during its processing. `/scratch1` is also ideal for non-parallel jobs with heavy I/O needs.

All data in the `/home` directory is permanent (not automatically deleted) and backed-up on a nightly basis.
If you lose data in the `/home` directory, it may be possible to recover it if it was previously backed up.

Data in the `/scratch1` directories are **not** backed up, and any data that are untouched for 30 days is automatically
removed from the `/scratch1` directories. Data that cannot easily be reproduced should **not** be stored in the `/scratch1` directories,
and any data that are not required should be removed as soon as possible.

See [this guide](https://www.palmetto.clemson.edu/palmetto/basic/storage/) on how to choose the appropriate filesystem for your work.

#### Storage Overview Video

<iframe src="https://drive.google.com/file/d/1kdzeLSjOxJKZhSYMjpKftPAvLcuUT1vg/preview" width="670" height="376" ></iframe>
<br />

#### Temporary reservations

All users may apply for temporary reservation
of the following resources:

1. Up to 150 TB of long-term storage
2. Up to 8.5 TB of fast SSD scratch space

All requests will be reviewed by Clemson University
Computational Advisory Team (CU-CAT).
Reservation requests can be made [here](https://citi.sites.clemson.edu/new-reservation/).

#### Purchased storage

Users or groups may also purchase storage in 1 TB increments.
For details about purchased storage, please contact the Palmetto support staff
(<ithelp@clemson.edu>, and include the word "Palmetto" in the subject line).

## Data transfer in and out of Palmetto

Data can be transferred in and out of Palmetto via the following recommended methods:

- Direct transfer using `login`: Data size smaller than 10GB.
- Direct transfer using `xfer01`: Data size between 10-100GB.
- Transfer using Globus: Data size larger than 100GB.

#### Direct transfer

On Windows machines, using the MobaXterm SSH client, the built-in file browser can be used (**SCP** tab of the side window).
Using the Upload (green arrow pointing up) and and Dowload (blue arrow pointing down) buttons at the top of the SCP tab,
you can easily transfer small files between Palmetto and your local computer.

**You can setup an SSH session to `xfer01-ext.clemson.edu` similar to how you login to `login.palmetto.clemson.edu`.**

<img src="../../images/basic/started/palmetto_02.png" style="width:300px">

On Unix systems, you can use the `scp` (secure copy) command to
perform file transfers. The general form of the `scp` command is:

```
$ scp <path_to_source> username@xfer01-ext.clemson.edu:<path_to_destination>
```

For example, here is the `scp` command to copy a file from the
current directory on your local machine to your
`/home/username` directory on Palmetto
(this command is entered into a terminal
when **not logged-in to Palmetto**):

```
$ scp myfile.txt username@xfer01-ext.clemson.edu:/home/username
```

... and to do the same in reverse,
i.e., copy from Palmetto to your local machine.
(again, from a terminal running on your local machine,
**not** on Palmetto):

```
$ scp username@xfer01-ext.clemson.edu:/home/username/myfile.txt .
```

The `.` represents the working directory on the local machine.

For folders, include the `-r` switch:

```
$ scp -r myfolder username@xfer01-ext.clemson.edu:/home/username
```

#### Transfer large files using Globus

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
    [https://app.globus.org/](https://app.globus.org/).

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
    In the figure below, the endpoint is named `grigori_mac`.

4.  As the second endpoint,
    choose `clemson#hpcdtn02.clemson.edu`.

5.  Make sure that you check the option to `verify file integrity after transfer` under the `Transfer & Sync Options`.

6.  You can now transfer files between any locations on your
    local machine and the Palmetto cluster.

<img src="../../images/basic/started/globus_screen.png" style="width:1000px">

#### MobaXTerm File Transfer Video

<iframe src="https://drive.google.com/file/d/1Ykg3hIkLH6Am44IQ7cT85NaygKrkW-AA/preview" width="670" height="376" ></iframe>
<br />

#### Mac/Globus File Transfer Video

<iframe src="https://drive.google.com/file/d/1fBZWxArbvb54s58ddF9Fj8xNsKRCGg-d/preview" width="670" height="376" ></iframe>
<br />

## Viewing all Palmetto's hardware

The login nodes `login001-login004` (the node that users first log-in to), are not meant for running computationally intensive tasks.
Instead, users must reserve hardware from the **compute nodes** of the cluster. Currently, Palmetto cluster has over 2040 compute
nodes. The hardware configuration of the different nodes is available in the file `/etc/hardware-table`:

```
[lngo@login001 ~]$ cat /etc/hardware-table

PALMETTO HARDWARE TABLE      Last updated:  Jun 15 2020

PHASE COUNT  MAKE   MODEL    CHIP(0)                CORES  RAM(1)    /local_scratch   Interconnect     GPUs  SSD

 0      6    HP     DL580    Intel Xeon    7542       24   505 GB(2)    99 GB         1ge               0     0
 0      5    Dell   R820     Intel Xeon    E5-4640    32   750 GB(2)   740 GB(12)     10ge              0     0
 0      1    HP     DL560    Intel Xeon    E5-4627v4  40   1.5 TB(2)   881 GB         10ge              0     0
 0      1    Dell   R830     Intel Xeon    E5-4627v4  40   1.0 TB(2)   880 GB         10ge              0     0
 0      1    HPE    DL560    Intel Xeon    6138G      80   1.5 TB(2)   3.6 TB         10ge              0     0
 0      1    HPE    DL560    Intel Xeon    6148G      80   1.5 TB(2)   3.6 TB         10ge              0     0
 0      1    HPE    DL560    Intel Xeon    6148G      80   1.5 TB(2)   745 GB(12)     10ge              0     0

 1a   116    Dell   R610     Intel Xeon    E5520       8    31 GB      220 GB         1g                0     0
 1b    40    Dell   R610     Intel Xeon    E5645      12    92 GB      220 GB         1g                0     0
 2a    30    Dell   R620     Intel Xeon    E5-2660    16   375 GB      2.7 TB         1g                0     0
 2b   134    Dell   PE1950   Intel Xeon    E5410       8    15 GB       37 GB         1g                0     0
 3    224    Sun    X2200    AMD   Opteron 2356        8    15 GB      193 GB         1g                0     0
 4    323    IBM    DX340    Intel Xeon    E5410       8    15 GB      111 GB         1g                0     0
 5a   310    Sun    X6250    Intel Xeon    L5420       8    31 GB       31 GB         1g                0     0
 5b     9    Sun    X4150    Intel Xeon    E5410       8    31 GB       99 GB         1g                0     0
 5c    19    Dell   R510     Intel Xeon    E5640       8    22 GB        7 TB         1g                0     0
 5d    20    Dell   R520     Intel Xeon    E5-2450    12    46 GB      2.7 TB         1g                0     0
 6     66    HP     DL165    AMD   Opteron 6176       24    46 GB      193 GB         1g                0     0

 7a    42    HP     SL230    Intel Xeon    E5-2665    16    62 GB      240 GB         56g, fdr, 10ge    0     0
 7b    12    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      240 GB         56g, fdr, 10ge    2(3)  0
 8a    71    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      900 GB         56g, fdr, 10ge    2(4)  300 GB(7)
 8b    57    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      420 GB         56g, fdr, 10ge    2(4)  0
 8c    88    Dell   PEC6220  Intel Xeon    E5-2665    16    62 GB      350 GB         56g, fdr, 10ge    0     0
 9     72    HP     SL250s   Intel Xeon    E5-2665    16   126 GB      420 GB         56g, fdr, 10ge    2(4)  0
10     80    HP     SL250s   Intel Xeon    E5-2670v2  20   126 GB      800 GB         56g, fdr, 10ge    2(4)  0
11a    40    HP     SL250s   Intel Xeon    E5-2670v2  20   126 GB      800 GB         56g, fdr, 10ge    2(6)  0
11b     4    HP     SL250s   Intel Xeon    E5-2670v2  20   126 GB      800 GB         56g, fdr, 10ge    0     0
12     30    Lenovo NX360M5  Intel Xeon    E5-2680v3  24   126 GB      800 GB         56g, fdr, 10ge    2(6)  0
13     24    Dell   C4130    Intel Xeon    E5-2680v3  24   126 GB      1.8 TB         56g, fdr, 10ge    2(6)  0
14     12    HPE    XL1X0R   Intel Xeon    E5-2680v3  24   126 GB      880 GB         56g, fdr, 10ge    2(6)  0
15     32    Dell   C4130    Intel Xeon    E5-2680v3  24   126 GB      1.8 TB         56g, fdr, 10ge    2(6)  0
16     40    Dell   C4130    Intel Xeon    E5-2680v4  28   126 GB      1.8 TB         56g, fdr, 10ge    2(8)  0
17     20    Dell   C4130    Intel Xeon    E5-2680v4  28   126 GB      1.8 TB         56g, fdr, 10ge    2(8)  0
18a     2    Dell   C4140    Intel Xeon    6148G      40   372 GB      1.9 TB(12)    100g, hdr, 10ge    4(9)  0
18b    65    Dell   R740     Intel Xeon    6148G      40   372 GB      1.8 TB        100g, hdr, 25ge    2(10) 0
18c    10    Dell   R740     Intel Xeon    6148G      40   748 GB      1.8 TB        100g, hdr, 25ge    2(10) 0
19a    28    Dell   R740     Intel Xeon    6248G      40   372 GB      1.8 TB        100g, hdr, 25ge    2(10) 0
19b     4    HPE    XL170    Intel Xeon    6252G      48   372 GB      1.8 TB         56g, fdr, 10ge    0     0

  *** PBS resource requests are always lowercase ***

(0) CHIP has 3 resources:   chip_manufacturer, chip_model, chip_type
(1) Leave 2 or 3GB for the operating system when requesting memory in PBS jobs
(2) Specify queue "bigmem" to access the large memory machines, only ncpus and mem are valid PBS resource requests
(3) 2 NVIDIA Tesla M2075 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2075"
(4) 2 NVIDIA Tesla K20m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k20"
(5) 2 NVIDIA Tesla M2070-Q cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2070q"
(6) 2 NVIDIA Tesla K40m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k40"
(7) Use resource request "ssd=true" to request a chunk with SSD in location /ssd1, /ssd2, and /ssd3 (100GB max each)
(8) 2 NVIDIA Tesla P100 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=p100"
(9) 4 NVIDIA Tesla V100 cards per node with NVLINK2, use resource request "ngpus=[1|2|3|4]" and "gpu_model=v100nv"
(10)2 NVIDIA Tesla V100 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=v100"
(11)Phase18a nodes contain NVMe storage for /local_scratch.
(12)local_scratch is housed entirely on SSD
```

The compute nodes are divided into "phases" (currently phases 0-19).
Each phase is composed of several nodes with identical configuration,
e.g., each node in phase 5a has 8 cores, 32 GB ram, 31 GB local disk space,
and 10 Gbps Myrinet interconnect.

A useful command on the login node is `whatsfree`, which gives information
about how many nodes from each phase are currently in use, free, or offlined
for maintenance.

```
[lngo@login001 ~]$ whatsfree

Mon Aug 03 2020 22:37:26

TOTAL NODES: 2102     NODES FREE: 2035     NODES OFFLINE: 14     NODES RESERVED: 0

PHASE 0    TOTAL =  16  FREE =  15  OFFLINE =   0  BIGMEM nodes: (6) 24cores/500GB, (5) 32cores/750GB, (3) 80cores/1.5TB, (1) 40cores/1.5TB, (1) 40cores/1TB, (1) 64cores/2TB
PHASE 1a   TOTAL = 117  FREE =  94  OFFLINE =   0  TYPE = Dell   R610    Intel Xeon  E5520,      8 cores,  31GB, 1g
PHASE 1b   TOTAL =  40  FREE =  40  OFFLINE =   0  TYPE = Dell   R610    Intel Xeon  E5645,     12 cores,  94GB, 1g
PHASE 2a   TOTAL =  30  FREE =  30  OFFLINE =   0  TYPE = Dell   R620    Intel Xeon  E5-2660    16 cores, 382GB, 1g
PHASE 2b   TOTAL = 162  FREE = 162  OFFLINE =   0  TYPE = Dell   PE1950  Intel Xeon  E5410,      8 cores,  15GB, 1g
PHASE 3    TOTAL = 224  FREE = 224  OFFLINE =   0  TYPE = Sun    X2200   AMD Opteron 2356,       8 cores,  15GB, 1g
PHASE 4    TOTAL = 319  FREE = 319  OFFLINE =   0  TYPE = IBM    DX340   Intel Xeon  E5410,      8 cores,  15GB, 1g
PHASE 5a   TOTAL = 308  FREE = 308  OFFLINE =   0  TYPE = Sun    X6250   Intel Xeon  L5420,      8 cores,  30GB, 1g
PHASE 5b   TOTAL =   9  FREE =   9  OFFLINE =   0  TYPE = Sun    X4150   Intel Xeon  E5410,      8 cores,  15GB, 1g
PHASE 5c   TOTAL =  34  FREE =  34  OFFLINE =   0  TYPE = Dell   R510    Intel Xeon  E5460,      8 cores,  23GB, 1g
PHASE 5d   TOTAL =  23  FREE =  23  OFFLINE =   0  TYPE = Dell   R520    Intel Xeon  E5-2450    12 cores,  46GB, 1g
PHASE 6    TOTAL =  65  FREE =  64  OFFLINE =   0  TYPE = HP     DL165   AMD Opteron 6176,      24 cores,  46GB, 1g
PHASE 7a   TOTAL =  42  FREE =  38  OFFLINE =   0  TYPE = HP     SL230   Intel Xeon  E5-2665,   16 cores,  62GB, FDR
PHASE 7b   TOTAL =  12  FREE =   0  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  62GB, FDR, M2075
PHASE 8a   TOTAL =  71  FREE =  61  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  62GB, FDR, K20, SSD
PHASE 8b   TOTAL =  57  FREE =  57  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  62GB, FDR, K20
PHASE 8c   TOTAL =  88  FREE =  78  OFFLINE =  10  TYPE = Dell   PEC6220 Intel Xeon  E5-2665,   16 cores,  62GB, 10ge
PHASE 9    TOTAL =  72  FREE =  69  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores, 125GB, FDR, K20, 10ge
PHASE 10   TOTAL =  80  FREE =  76  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 125GB, FDR, K20, 10ge
PHASE 11a  TOTAL =  40  FREE =  39  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 125GB, FDR, K40, 10ge
PHASE 11b  TOTAL =   4  FREE =   4  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 125GB, FDR, Phi, 10ge
PHASE 12   TOTAL =  30  FREE =  30  OFFLINE =   0  TYPE = Lenovo MX360M5 Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, K40, 10ge
PHASE 13   TOTAL =  24  FREE =  24  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, K40, 10ge
PHASE 14   TOTAL =  12  FREE =  12  OFFLINE =   0  TYPE = HP     XL190r  Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, K40, 10ge
PHASE 15   TOTAL =  32  FREE =  32  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, K40, 10ge
PHASE 16   TOTAL =  40  FREE =  37  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v4, 28 cores, 125GB, FDR, P100, 10ge
PHASE 17   TOTAL =  20  FREE =  20  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v4, 28 cores, 125GB, FDR, P100, 10ge
PHASE 18a  TOTAL =   2  FREE =   0  OFFLINE =   0  TYPE = Dell   C4140   Intel Xeon  6148G,     40 cores, 372GB, HDR, V100nv, 10ge
PHASE 18b  TOTAL =  65  FREE =  55  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6148G,     40 cores, 372GB, HDR, V100, 25ge
PHASE 18c  TOTAL =  10  FREE =  10  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6148G,     40 cores, 748GB, HDR, V100, 25ge
PHASE 19a  TOTAL =  28  FREE =  24  OFFLINE =   4  TYPE = Dell   R740    Intel Xeon  6248G,     40 cores, 372GB, HDR, V100, 25ge
PHASE 19b  TOTAL =   4  FREE =   3  OFFLINE =   0  TYPE = HPE    XL170   Intel Xeon  6252G,     48 cores, 372GB, 10ge
PHASE 20   TOTAL =  22  FREE =  22  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6238R,     56 cores, 372GB, HDR, V100S, 25ge

 NOTE: Your job will land on the oldest phase that satisfies your PBS resource requests.
```

Later sections of this guide will describe how to submit **jobs** to
the cluster, i.e., reserve compute nodes for running computational tasks.

## Checking and using available software

The Palmetto cluster provides a limited number of packages (including site-licensed packages),
that can be used by all Palmetto users. These packages are available as **modules**,
and must be activated/deactivated using the `module` command:

| Command                      | Purpose                                         |
| ---------------------------- | ----------------------------------------------- |
| `module avail`               | List all packages available (on current system) |
| `module add package/version` | Add a package to your current shell environment |
| `module list`                | List packages you have loaded                   |
| `module rm package/version`  | Remove a currently loaded package               |
| `module purge`               | Remove _all_ currently loaded packages          |

For example, to load the GCC (v9.3.0), CUDA Toolkit (v10.2.89)
and OpenMPI (v3.1.6) modules, you can use the command:

```
[lngo@login001 ~]$ module add gcc/9.3.0-gcc/8.3.1 cuda/10.2.89-gcc/8.3.1 openmpi/3.1.6-gcc/8.3.1-cuda10_2-ucx
```

Then, check the loaded modules and version of `gcc`:

```
[lngo@login001 ~]$ module list

Currently Loaded Modules:
  1) gmp/6.1.2-gcc/8.3.1   2) gcc/9.3.0-gcc/8.3.1   3) cuda/10.2.89-gcc/8.3.1   4) openmpi/3.1.6-gcc/8.3.1-cuda10_2-ucx

[lngo@login001 ~]$ gcc --version
gcc (Spack GCC) 9.3.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

[lngo@login001 ~]$
```

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

```
$ echo $PATH
$ module add anaconda3/2019.10-gcc/8.3.1
$ echo $PATH
```

## Job submission and control on Palmetto

The Palmetto cluster uses the Portable Batch Scheduling system (PBS)
to manage jobs. Here are some basic PBS commands
for submitting, querying and deleting jobs:

| Command                       | Action                                                                    |
| ----------------------------- | ------------------------------------------------------------------------- |
| `qsub -I`                     | Submit an interactive job (reserves 1 core, 1gb RAM, 30 minutes walltime) |
| `qsub xyz.pbs`                | Submit the job script `xyz.pbs`                                           |
| `qstat <job id>`              | Check the status of the job with given job ID                             |
| `qstat -u <username>`         | Check the status of all jobs submitted by given username                  |
| `qstat -xf <job id>`          | Check detailed information for job with given job ID                      |
| `qsub -q <queuename> xyz.pbs` | Submit to queue `queuename`                                               |
| `qdel <job id>`               | Delete the job (queued or running) with given job ID                      |
| `qpeek <job id>`              | "Peek" at the standard output from a running job                          |
| `qdel -Wforce <job id>`       | Use when job not responding to just `qdel`                                |

For more details and more advanced commands for submitting and controlling jobs,
please refer to the [PBS Professional User's Guide](http://www.pbsworks.com/pdfs/PBSUserGuide14.2.pdf).

It should be noted that for Palmetto, user will need to specify `interconnect` in order to  
get an allocation, unless you submit a default `qsub -I` with no parameters.

### Starting an interactive job

An interactive job can be started using the `qsub` command.
Here is an example of an interactive job:

```
[username@login001 ~]$ qsub -I -l select=1:ncpus=2:mem=4gb:interconnect=1g,walltime=4:00:00
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 8730.pbs02 to start
qsub: job 8730.pbs02 ready

[username@node0021 ~]$ module add anaconda3/2019.10-gcc/8.3.1
[username@node0021 ~]$ python runsim.py
.
.
.
[username@node0021 ~]$ exit
[username@login001 ~]$
```

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

```
#PBS -N example
#PBS -l select=1:ncpus=1:mem=2gb:interconnect=1g,walltime=00:10:00

module add gcc/9.3.0-gcc/8.3.1

cd /home/username
echo Hello World from `hostname`
sleep 60
```

After saving the above file, you can submit the batch job using `qsub`:

```
[username@login001 ~]$ qsub example.pbs
8738.pbs02
```

The returned job ID can be used to query the status of the job (using `qstat`)
(or delete it using `qdel`):

```
[username@login001 ~]$ qstat 8738.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
8738.pbs02        example          username           00:00:00 R c1_solo
```

Once the job is completed, you will see the files `example.o8738` (containing output if any)
and `example.e8738` (containing errors if any) from your job.

```
[username@login001 ~]$ cat example.o8738
Hello World from node0230.palmetto.clemson.edu
```

### PBS job options

The following switches can be used either
with `qsub` on the command line,
or with a `#PBS` directive in a batch script.

| Parameter | Purpose                                                                                                                                                                                                                 | Example                                       |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| `-N`      | Job name (7 characters)                                                                                                                                                                                                 | `-N maxrun1`                                  |
| `-l`      | Job limits (lowercase L), hardware & other requirements for job.                                                                                                                                                        | `-l select=1:ncpus=8:mem=1gb:interconnect=1g` |
| `-q`      | Queue to direct this job to (`work1` is the default, `supabad` is an example of specific research group's job queue)                                                                                                    | `-q supabad`                                  |
| `-o`      | Path to stdout file for this job (environment variables are not accepted here)                                                                                                                                          | `-o stdout.txt`                               |
| `-e`      | Path to stderr file for this job (environment variables are not accepted here)                                                                                                                                          | `-e stderr.txt`                               |
| `-m`      | mail event: Email from the PBS server with flag **a**bort\ **b**egin\ **e**nd \ or **n**o mail for job's notification.                                                                                                  | `-m abe`                                      |
| `-M`      | Specify list of user to whom mail about the job is sent. The user list argument is of the form: [user[@host],user[@host],...]. If **-M** is not used and **-m** is specified, PBS will send email to userid@clemson.edu | `-M user1@domain1.com,user2@domain2.com`      |
| `-j oe`   | Join the output and error streams and write to a single file                                                                                                                                                            | `-j oe`                                       |
| `-r n`    | Ask PBS **not** to restart the job if it's failed                                                                                                                                                                       | `-r n`                                        |

For example, in a batch script:

```
#PBS -N hydrogen
#PBS -l select=1:ncpus=24:mem=200gb:interconnect=1g,walltime=4:00:00
#PBS -q bigmem
#PBS -m abe
#PBS -M userid@domain.com
#PBS -j oe
```

And in an interactive job request on the command line:

```
$ qsub -I -N hydrogen -q bigmem -j oe -l select=1:ncpus=24:mem=200gb:interconnect=1g,walltime=4:00:00
```

For more detailed information, please take a look at:

```
$man qsub
```

### Resource limits specification

The `-l` switch provided to `qsub` or along with the `#PBS` directive
can be used to specify the amount and kind of compute hardware (cores, memory, GPUs, interconnect, etc.,),
its location, i.e., the node(s) and phase from which to request hardware,

| Option     | Purpose                                                                                                                                             |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `select`   | Number of chunks and resources per chunk. Two or more "chunks" can be placed on a single node, but a single "chunk" cannot span more than one node. |
| `walltime` | Expected wall time of job (job is terminated after this time)                                                                                       |
| `place`    | Controls the placement of the different chunks                                                                                                      |

Here are some examples of resource limits specification:

```
-l select=1:ncpus=8:chip_model=opteron:interconnect=1g
-l select=1:ncpus=16:chip_type=e5-2665:interconnect=10ge:mem=62gb,walltime=16:00:00
-l select=1:ncpus=8:chip_type=2356:interconnect=10ge:mem=15gb
-l select=1:ncpus=4:mem=15gb:ngpus=2:interconnect=fdr,walltime=00:20:00
-l select=1:ncpus=4:mem=15gb:ngpus=1:gpu_model=k40:interconnect=fdr,walltime=00:20:00
-l select=1:ncpus=2:mem=15gb:host=node1479:interconnect=1g,walltime=00:20:00
-l select=2:ncpus=2:mem=15gb:interconnect=1g,walltime=00:20:00,place=scatter    # force each chunk to be on a different node
-l select=2:ncpus=2:mem=15gb:interconnect=10g,walltime=00:20:00,place=pack       # force each chunk to be on the same node
-l select=2:ncpus=2:mem=15gb:phase=9:interconnect=fdr,walltime=00:20:00          # force each chunk to be on the same phase
-l select=1:ncpus=16:mpiprocs=16:mem=15gb:interconnect=hdr,walltime=00:20:00     # specify number of mpi processing

```

and examples of options you can use in the job limit specification:

```
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
interconnect=10ge   (10 Gbps Ethernet)
interconnect=56g    (56 Gbps FDR InfiniBand, same as fdr)
interconnect=fdr    (56 Gbps FDR InfiniBand, same as 56g)
ssd=true   			(Use a node with an SSD hard drive)
```

### Querying job information and deleting jobs

The `qstat` command can be used
to query the status of a particular job:

```
$ qstat 7600424.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
7600424.pbs02     pi-mpi           username           00:00:00 R c1_single
```

To list the job IDs and status of all your jobs,
you can use the `-u` switch:

```
$ qstat -u username

pbs02:
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
7600567.pbs02   username  c1_singl pi-mpi-1     1382   4   8    4gb 00:05 R 00:00
7600569.pbs02   username  c1_singl pi-mpi-2    20258   4   8    4gb 00:05 R 00:00
7600570.pbs02   username  c1_singl pi-mpi-3     2457   4   8    4gb 00:05 R 00:00
```

Once a job has finished running,
`qstat -xf` can be used to obtain detailed job information:

```
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
```

Similarly, to get detailed information about
a running job, you can use `qstat -f`.

To delete a job (whether in queued, running or error status),
you can use the `qdel` command.

```
$ qdel 7600424.pbs02
```

### Job limits on Palmetto

#### Walltime

Jobs running in phases 1-6 of the cluster (nodes with interconnect `1g`)
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

2. IB queues (`c2_` queues): jobs submitted to run the newer hardware (phases 7 and up)
   will be forwarded to these queues.

3. GPU queues (`gpu_` queues): jobs that
   [request GPUs]({{site.baseurl}}/userguide_howto_use_gpus.html)
   will be forwarded to these queues.

4. bigmem queue: jobs submitted to the large-memory machines (phase 0).

Each execution queue has its own limits for
how many jobs can be running at one time,
and how many jobs can
be waiting in that execution queue.
The maximum number of running jobs per user in
execution queues may vary
throughout the day depending on cluster load.
Users can see what the current limits
are using the `checkqueuecfg` command:

```
[lngo@login001 ~]$ checkqueuecfg

1G QUEUES     min_cores_per_job  max_cores_per_job   max_mem_per_queue  max_jobs_per_queue   max_walltime
c1_solo                       1                  1               800gb                 100      168:00:00
c1_single                     2                 24              1200gb                  10      168:00:00
c1_tiny                      25                128              5120gb                   5      168:00:00
c1_small                    129                512              4096gb                   1      168:00:00
c1_medium                   513               2048                 0gb                   0      168:00:00
c1_large                   2049               4096                 0gb                   0      168:00:00

IB QUEUES     min_cores_per_job  max_cores_per_job   max_mem_per_queue  max_jobs_per_queue   max_walltime
c2_single                     1                 40              4000gb                  10       72:00:00
c2_tiny                      41                200              6400gb                   2       72:00:00
c2_small                    201                512              6144gb                   1       72:00:00
c2_medium                   513               2048             16384gb                   1       72:00:00
c2_large                   2049               4096                 0gb                   0       72:00:00

GPU QUEUES     min_gpus_per_job   max_gpus_per_job  min_cores_per_job  max_cores_per_job   max_mem_per_queue  max_jobs_per_queue   max_walltime
gpu_small                     1                  4                  1                 96              1440gb                   5       72:00:00
gpu_medium                    5                 16                  1                256              3072gb                   2       72:00:00
gpu_large                    17                256                  1               2048              6144gb                   1       72:00:00

VGPU QUEUES   min_vgpus_per_job  max_vgpus_per_job  min_cores_per_job  max_cores_per_job   max_mem_per_queue  max_jobs_per_queue   max_walltime
vgpu_small                    1                  1                  1                  4                 0gb                   0       72:00:00

SMP QUEUE     min_cores  max_cores   max_jobs   max_walltime
bigmem                1         80          1      168:00:00


   'max_mem' is the maximum amount of memory all your jobs in this queue can
   consume at any one time.  For example, if the max_mem for the solo queue
   is 4000gb, and your solo jobs each need 10gb, then you can run a
   maximum number of 4000/10 = 400 jobs in the solo queue, even though the
   current max_jobs setting for the solo queue may be set higher than 400.


[lngo@login001 ~]$

```

The `qstat` command tells you which of the execution queues
your job is forwarded to. For example, here is an interactive
job requesting 8 CPU cores, a K40 GPU, and 32gb RAM:

```
$ qsub -I -l select=1:ncpus=8:ngpus=1:gpu_model=k40:mem=32gb,walltime=2:00:00
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 9567792.pbs02 to start
```

We see from `qstat` that the job request is forward to
the `c2_single` queue:

```
[username@login001 ~]$ qstat 9567792.pbs02
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
9567792.pbs02     STDIN            username                  0 Q c2_single
```

From the output of `checkqueuecfg` above,
we see that each user can have a maximum of **5** running jobs in this queue.

### Example PBS scripts

A list of example PBS scripts for submitting
jobs to the Palmetto cluster can be found
[here](https://github.com/clemsonciti/palmetto-examples).
