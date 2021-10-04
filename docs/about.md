## Overview

<img src="../images/about/palmetto_front_view.png" style="width:500px">

Palmetto is Clemson University's primary high-performance computing (HPC) resource;
heavily utilized by researchers, students, faculty, and staff from a broad range of disciplines.

Currently, Palmetto is comprised of 2115 compute nodes (totalling 32600 CPU cores),
and features:

* 2115 compute nodes, totalling 32600 cores
* 639 nodes are equipped with 2x NVIDIA Tesla GPUs, with a total of 1278 GPUs in the cluster; out of these, 34 nodes each have 2x NVIDIA Tesla A100 GPUs
* 3 nodes with Intel Phi co-processors (2 per node)
* 15 large-memory nodes (with 0.75 TB - 1.5 TB of memory); in addition, 604 nodes have at least 128 GB of RAM
* 100 GB of personal space (backed up daily for 42 days) for each user
* 2.2 PB of scratch storage space for computing and a burst-buffer
* 10 and 25 Gbps Ethernet, and 56 and 100 Gbps Infiniband networks
<!--* ranked 9th among the public academic institutions in the US (and 392nd overall among all world-wide supercomputers) on Top500 list, as of November 2019-->
* benchmarked at 1.4 PFlops (44,016 cores from Infiniband part of Palmetto)
* the cluster is 100% battery-backed

## Compute Hardware

<img src="../images/about/palmetto_nodes_closeup.png" style="width:500px">

The cluster is divided into several "phases";
the basic hardware configuration (node count, cores, RAM)
is given below. For more detailed and up-to-date information,
you can view the file `/etc/hardware-table` after logging in.

### Ethernet phases

Phases 0 through 6 of the cluster consist of older hardware
with 10 Gbps Ethernet interconnect. Maximum run time for a single task is limited to 168 hours.

Phase  	| Node count 	| Cores | RAM
--------|---------------|-------|-------
1a  		| 117    	| 8         	| 31 GB
1b      | 43  		| 12        	| 92 GB
2a    	| 45    		| 16    	| 382 GB
2b    	| 159    		| 8     	| 15 GB
2c      | 88        | 16      | 62 GB
3    	| 185    		| 8    	| 15 GB
4    	| 318    		| 8    	| 15 GB
5a   	| 271    		| 8    	| 31 GB
5b     	| 9      		| 8    	| 31 GB
5c   	| 36    		| 8    	| 22 GB
5d     	| 23      		| 12    	| 46 GB
6     	| 65    		| 24    | 46 GB


### Infiniband phases
Phases 7-27 of the cluster consist of newer hardware
with 56 Gbps Infiniband interconnect (Phases 7-17) and 100 Gbps Infiniband interconnect (Phases 18-27). Maximum run time for a single task is limited to 72 hours.

Phase  	| Node count 	| Cores | RAM
--------|---------------|-------|-------
7a		| 42 			| 16   	| 62 GB
7b    	| 12    		| 16   	| 62 GB
8a    	| 71    		| 16   	| 62 GB
8b    	| 57    		| 16   	| 62 GB
9     	| 72     		| 16   	| 126 GB
10     	| 80    		| 20   	| 126 GB
11a		| 41			| 20	| 126 GB
11b		| 3				| 20	| 126 GB
11c		| 41				| 16	| 250 GB
12		| 29			| 24	| 126 GB
13		| 24			| 24	| 126 GB
14		| 12			| 24	| 126 GB
15		| 32			| 24	| 126 GB
16		| 40			| 28	| 126 GB
17		| 20			| 28	| 126 GB
18a		| 2			| 40	| 372 GB
18b		| 65			| 40	| 372 GB
18c		| 10			| 40	| 748 GB
19a		| 28			| 40	| 372 GB
19b		| 4			| 48	| 372 GB
19c   | 2     | 40  | 372 GB
20   | 22     | 56  | 372 GB
27   | 34     | 56  | 372 GB



### GPUs

There are 639 nodes (phases 8a-11a, 12-19a, 20, 27)
on Palmetto equipped with NVIDIA Tesla GPUs
(K20m, K40m, P100, V100, A100).

### Intel Xeon Phi accelerators

3 nodes (phase 11b) are equipped with Intel Phi co-processors (2 per node).

### Big-memory nodes

Phase 0 consists of 6 "bigmem" machines with large core count and RAM (750 GB to 1.5 TB).

Phase  	| Node count 	| Cores 	| RAM
--------|---------------|-----------|---
0a		   | 3 			| 24   		| 1 TB
0b		   | 5 			| 32   		| 750 GB
0c    	 | 1	    | 40   		| 1 TB
0d		   | 2 			| 36   		| 1.5 TB
0e		   | 1 			| 40   		| 1.5 TB
0f		   | 3 			| 80   		| 1.5 TB

## Storage

Various options for storing data
(on temporary and permanent basis) are available to researchers
using Palmetto:

Location        |	Available space                     | Notes
----------------|---------------------------------------|---------------------------------------------------------------------------
`/home`         |   100 GB per user                     | Backed-up nightly, permanent storage space accessible from all nodes
`/scratch1`     |   2PB shared by all users             | Not backed up, temporary work space accessible from all nodes, BeeGFS Parallel File Sytem
`/scratch2`     |   190 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, ZFS
`/local_scratch`|   Varies between nodes (99GB-800GB)   | Per-node temporary work space, accessible only for the lifetime of job

Additional high-performance and backed-up storage may be purchased
for group usage. Please visit the [Owner's Guide](https://www.palmetto.clemson.edu/palmetto/owner/index.html).

## Job scheduling

The Palmetto cluster uses the
[Portable Batch Scheduling system (PBS)](http://www.pbsworks.com/PBSProduct.aspx?n=PBS-Professional&c=Overview-and-Capabilities) to schedule
jobs.

## Condominium model

Palmetto cluster operates in a condominium model which allows faculty to
purchase immediate access to compute nodes on the cluster.
More information can be found in the [Owner's Guide](https://www.palmetto.clemson.edu/palmetto/owner/index.html).


## Acknowledging Palmetto Cluster

We would appreciate if all publications that include results generated using the Palmetto cluster
include a short statement in the Acknowledgment section.
As an example, the acknowledgment may look like this:

**Clemson University is acknowledged for generous allotment of compute time on Palmetto cluster.**
