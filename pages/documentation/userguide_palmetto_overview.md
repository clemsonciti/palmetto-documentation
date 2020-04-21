---
title: About Palmetto Cluster
keywords: [hardware,storage,condo,purchase]
sidebar: documentation_sidebar
permalink: userguide_palmetto_overview.html
---

## Overview

<img src="{{site.baseurl}}/images/palmetto-front-view.png" style="width:500px">

Palmetto is Clemson University's primary high-performance computing (HPC) resource;
heavily utilized by researchers, students, faculty, and staff from a broad range of disciplines.

Currently, Palmetto is comprised of 2021 compute nodes (totalling 23072 CPU cores),
and features:

* 2079 compute nodes, totalling 28832 cores
* 595 nodes are equipped with 2x NVIDIA Tesla GPUs, with a total of 1194 GPUs in the cluster; 103 nodes each have 2x NVIDIA Tesla V100 GPUs
* 4 nodes with Intel Phi co-processors (2 per node)
* 17 large-memory nodes (with 0.5 TB - 2.0 TB of memory); in addition, 480 nodes have at least 128 GB of RAM
* 100 GB of personal space (backed up daily for 42 days) for each user
* 726 TB of scratch storage space for computing and a burst-buffer
* 10 and 25 Gbps Ethernet, and 56 and 100 Gbps Infiniband networks
* ranked 9th among the public academic institutions in the US (and 392nd overall among all world-wide supercomputers) on Top500 list, as of November 2019
* benchmarked at 1.4 PFlops (44,016 cores from Infiniband part of Palmetto)
* the cluster is 100% battery-backed

## Compute Hardware

<img src="{{site.baseurl}}/images/palmetto-nodes-closeup.png" style="width:500px">

The cluster is divided into several "phases";
the basic hardware configuration (node count, cores, RAM)
is given below. For more detailed and up-to-date information,
you can view the file `/etc/hardware-table` after logging in.

### Ethernet phases

Phases 0 through 6 of the cluster consist of older hardware
with 10 Gbps Ethernet interconnect. Maximum run time for a single task is limited to 168 hours.

Phase  	| Node count 	| Cores | RAM
--------|---------------|-------|-------
1a  		| 116    	| 8         	| 31 GB
1b      | 42  		| 12        	| 92 GB
2a    	| 10    		| 16    	| 382 GB
2b    	| 134    		| 8     	| 15 GB
3    	| 224    		| 8    	| 15 GB
4    	| 323    		| 8    	| 15 GB
5a   	| 310    		| 8    	| 31 GB
5b     	| 9      		| 8    	| 31 GB
5c   	| 19    		| 8    	| 22 GB
5d     	| 20      		| 12    	| 46 GB
6     	| 66    		| 24    | 46 GB

### Infiniband phases
Phases 7-19 of the cluster consist of newer hardware
with 56 Gbps Infiniband interconnect (Phases 7-17) and 100 Gbps Infiniband interconnect (Phases 18-19). Maximum run time for a single task is limited to 72 hours.

Phase  	| Node count 	| Cores | RAM
--------|---------------|-------|-------
7a		| 42 			| 16   	| 62 GB
7b    	| 12    		| 16   	| 62 GB
8a    	| 71    		| 16   	| 62 GB
8b    	| 57    		| 16   	| 62 GB
8c 		| 88    		| 16   	| 62 GB
9     	| 72     		| 16   	| 126 GB
10     	| 80    		| 20   	| 126 GB
11a		| 40			| 20	| 126 GB
11b		| 4				| 20	| 126 GB
12		| 30			| 24	| 126 GB
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



### GPUs

There are 595 nodes (phases 7a-8b, 9-11a, 12-19a)
on Palmetto equipped with NVIDIA Tesla GPUs
(M2075, K20m, M2070q, K40m, P100, V100).

### Intel Xeon Phi accelerators

4 nodes (phase 11b) are equipped with Intel Phi co-processors (2 per node).

### Big-memory nodes

Phase 0 consists of 6 "bigmem" machines with large core count and RAM (505 GB to 2 TB).

Phase  	| Node count 	| Cores 	| RAM
--------|---------------|-----------|---
0		   | 6 			| 24   		| 505 GB
0    	 | 1	    | 64   		| 2 TB
0		   | 5 			| 32   		| 750 GB
0    	 | 1	    | 40   		| 1.5 TB
0		   | 1 			| 40   		| 1 TB
0    	 | 3	    | 80   		| 1.5 TB

## Storage

Various options for storing data
(on temporary and permanent basis) are available to researchers
using Palmetto:

Location        |	Available space                     | Notes
----------------|---------------------------------------|---------------------------------------------------------------------------
`/home`         |   100 GB per user                     | Backed-up nightly, permanent storage space accessible from all nodes
`/scratch1`     |   233 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, BeeGFS Parallel File Sytem
`/scratch2`     |   160 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, XFS
`/scratch3`     |   129 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, ZFS
`/scratch4`     |   174 TB shared by all users          | Not backed up, temporary work space accessible from all nodes, BeeGFS Parallel File Sytem
`/local_scratch`|   Varies between nodes (99GB-800GB)   | Per-node temporary work space, accessible only for the lifetime of job

Additional high-performance and backed-up storage may be purchased
for group usage. Please visit [http://citi.clemson.edu/infrastructure](http://citi.clemson.edu/infrastructure)
for details and pricing.

## Job scheduling

The Palmetto cluster uses the
[Portable Batch Scheduling system (PBS)](http://www.pbsworks.com/PBSProduct.aspx?n=PBS-Professional&c=Overview-and-Capabilities) to schedule
jobs.

## Condominium model

Palmetto cluster operates in a condominium model which allows faculty to
purchase immediate access to compute nodes on the cluster.
More information can be found in the [Owner's Guide]({{site.baseurl}}/owners.html).
