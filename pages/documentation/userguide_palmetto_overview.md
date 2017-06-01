---
title: Overview of Palmetto Cluster
keywords: overview
sidebar: documentation_sidebar
permalink: userguide_palmetto_overview.html
---

## Compute hardware

Palmetto is a heterogenous cluster composed
of 2021 compute nodes (23072 cores),
with varying core counts, RAM, interconnect, etc.,
After logging in, the command

~~~
$ cat /etc/hardware-table
~~~

presents an overview of available compute hardware on the cluster:

~~~
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
(2) Specify queue "bigmem" to access the large memory machines, only ncpus and mem are valid PBS resource requests
(3) 2 NVIDIA Tesla M2075 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2075"
(4) 2 NVIDIA Tesla K20m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k20"
(5) 2 NVIDIA Tesla M2070-Q cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2070q"
(6) 2 NVIDIA Tesla K40m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k40"
(7) Use resource request "ssd=true" to request a chunk with SSD in location /ssd1, /ssd2, and /ssd3 (100GB max each)
(8) Use resource request "nphis=[1|2]" to request phi nodes, the model is Xeon 7120p
~~~

### Myrinet phases

Phases 0-6 of the cluster consist of older hardware
with 10 Gbps Myrinet interconnect.

### Infiniband phases

Phases 7-15 of the cluster consist of newer hardware
with 56 Gbpbs Infiniband interconnect,
and additionally 10 Gbps Ethernet for phases 9-15.

### GPUs

There are 386 nodes on Palmetto equipped with NVIDIA Tesla GPUs:
280 nodes with NVIDIA K20 GPUs (2 per node),
and 106 nodes with NVIDIA K40 GPUs (2 per node).

### Intel Xeon Phi accelerators

4 nodes are equipped with Intel Phi co-processors (2 per node).

### Big-memory nodes

Phase 0 consists of 6 "bigmem" machines with large core count and RAM (505 GB/2 TB).

## Storage

Various options for storing data
(on temporary and permanent basis) are available to researchers
using Palmetto:

Location        |	Available space                     | Notes
----------------|---------------------------------------|---------------------------------------------------------------------------
`/home`         |   100 GB per user                     | Backed-up nightly, permanent storage space accessible from all nodes
`/scratch1`     |   233 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, OrangeFS Parallel File Sytem
`/scratch2`     |   160 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, XFS
`/scratch3`     |   129 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, ZFS
`/local_scratch`|   Varies between nodes (99GB-800GB)   | Per-node temporary work space, accessible only for the lifetime of job

Additional high-performance and backed-up storage may be purchased
for group usage. Please visit http://citi.clemson.edu/infrastructure
for details and pricing.

## Job scheduling

The Palmetto cluster uses the
[Portable Batch Scheduling system (PBS)](http://www.pbsworks.com/PBSProduct.aspx?n=PBS-Professional&c=Overview-and-Capabilities) to schedule
jobs.

## Condominium model

Palmetto cluster operates in a condominium model which allows faculty to
purchase immediate access to compute nodes on the cluster.
By purchasing a compute node, faculty get priority to use an
equivalent hardware across all new phases of the cluster.
All unused compute cycles are made available for general Palmetto users.
Owners may preempt other users making the hardware they purchased immediately available.
Purchased nodes are available to faculty for a period of 4 years,
after that the priority to use them expires.

Being an owner allows users to:

* have immediate access to the amount of compute hardware they have purchased by preempting other users
* have a dedicated group on Palmetto cluster
* invite external collaborators (not associated with Clemson) to use their purchased resources
* have extended maximum time for a single task up to 336 hours (14 days)

Please see https://citi.sites.clemson.edu/infrastructure/ for details and pricing.
For more information about the condominium model and purchasing Palmetto nodes,
including Palmetto nodes on grants please contact Jeronica Williams
(jeronic@clemson.edu) or Marcin Ziolkowski (zziolko@clemson.edu).
