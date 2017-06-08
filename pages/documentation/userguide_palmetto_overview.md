---
title: About Palmetto Cluster
keywords: overview
sidebar: documentation_sidebar
permalink: userguide_palmetto_overview.html
---

## Overview

<img src="{{site.baseurl}}/images/palmetto-front-view.png" style="width:500px">

Palmetto is Clemson University's primary high-performance computing (HPC) resource;
heavily utilized by researchers, students, faculty, and staff from a broad range of disciplines. 

Currently, Palmetto is comprised of 2021 compute nodes (totalling 23072 CPU cores),
and features:

* 2021 compute nodes, totalling 23072 cores
* 386 nodes equipped with NVIDIA Tesla GPUs: 280 nodes with NVIDIA K20 GPUs (2 per node), 106 nodes with NVIDIA K40 GPUs (2 per node)
* 4 nodes with Intel Phi co-processors (2 per node)
* 6 large memory nodes (5 with 505GB, 1 with 2TB), 262 nodes with 128GB of memory
* 100GB of personal space (backed up daily for 42 days) for each user
* "unlimited" scratch storage for temporary files
* 10 Gbps Ethernet, 10 Gbps Myrinet and 56Gbps Infiniband networks
* maximum run time for a single task limited to 72 hours (Infiniband nodes) or 168 hours (Myrinet nodes)
* ranked 4th among the public academic institutions in the US on Top500 list (155 on Top500)
* benchmarked at 814.4 TFlops (17,372 cores from Infiniband part of Palmetto)

## Compute Hardware

<img src="{{site.baseurl}}/images/palmetto-nodes-closeup.png" style="width:500px">

The cluster is divided into several "phases";
the basic hardware configuration (node count, cores, RAM)
is given below. For more detailed and up-to-date information,
you can view the file `/etc/hardware-table` after logging in.

### Myrinet phases

About 1400 nodes (Phases 0-6 of) the cluster consist of older hardware
with 10 Gbps Myrinet interconnect.

Phase  	| Node count 	| Cores | RAM
--------|---------------|-------|-------
1  		| 191    		| 8    	| 12 GB
2    	| 243    		| 8    	| 12 GB
3    	| 234    		| 8    	| 16 GB
4    	| 329    		| 8    	| 16 GB
5a   	| 370    		| 8    	| 32 GB
5b     	| 9      		| 8    	| 16 GB
6     	| 68    		| 24    | 48 GB

### Infiniband phases

Phase  	| Node count 	| Cores | RAM
--------|---------------|-------|-------
7a		| 42 			| 16   	| 64 GB
7b    	| 12    		| 16   	| 64 GB
8a    	| 71    		| 16   	| 128 GB
8b    	| 57    		| 16   	| 128 GB
8c 		| 68    		| 16   	| 128 GB
9     	| 72     		| 16   	| 128 GB
10     	| 80    		| 20   	| 128 GB
11a		| 40			| 20	| 128 GB
11b		| 4				| 20	| 128 GB
12		| 30			| 24	| 128 GB
13		| 24			| 24	| 128 GB
14		| 12			| 24	| 128 GB
15		| 32			| 24	| 128 GB

Phases 7-15 of the cluster consist of newer hardware
with 56 Gbpbs Infiniband interconnect,
and additionally 10 Gbps Ethernet for phases 9-15.

### GPUs

There are about 380 nodes (phases 7a-8b, 9-11a, 12-15)
on Palmetto equipped with NVIDIA Tesla GPUs
(M2075, K20m, M2070q and K40m).

### Intel Xeon Phi accelerators

4 nodes (phase 11b) are equipped with Intel Phi co-processors (2 per node).

### Big-memory nodes

Phase 0 consists of 6 "bigmem" machines with large core count and RAM (505 GB/2 TB).

Phase  	| Node count 	| Cores 	| RAM
--------|---------------|-----------|---
0		| 5 			| 24   		| 505 GB
0    	| 1	    		| 64   		| 2 TB

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
