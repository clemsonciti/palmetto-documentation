---
title: Owner's Guide
keywords: [owners, condo]
sidebar: documentation_sidebar
permalink: /owners.html
---

## Purchasing Palmetto Compute Resources

Clemson University faculty or staff may purchase immediate access to
Palmetto compute nodes via the "condo model".
Such "owners" enjoy the following benefits:

1. 	Get immediate access to the purchased compute resources
2. 	Submit jobs with extended walltime using purchased compute resources (336 hours)
3.  Grant access to purchased compute resources for other
	Clemson students, faculty or staff
4.	Request access for external collaborators (unaffiliated with Clemson University)
	to purchased compute resource

### Condominium model

In the condo model model, faculty/staff purchase "pre-emption" units,
representing hardware with configuration equivalent to the newest
"phase" of compute nodes on the cluster.

An owner queue is created to represent the
resources purchased.
Depending on the hardware configuration of the current phase as compared
with any previous purchases made by this owner,
the owner may be assigned an additional queue
or may have an existing queue expanded in capacity.

When an owner submits a job to their owner queue, the job will automatically run on the
purchased resources represented by their owner queue. Owner jobs:

1. are given priority attention by the job scheduler before any job submitted by non-owners,
2. may preempt with a 120 second delay as needed any general jobs running on the requested resources, and
3. cannot themselves be preempted.

Preempted jobs are automatically requeued.
Owner jobs have a maximum walltime of 336 hours
(2 weeks).

### Current compute hardware and pricing

The node configuration currently available for purchase is:

* 2 x Intel Xeon E5-2680v4 "Broadwell" @2.4 GHz (for a total of 28 cores)
* 2 x NVIDIA Tesla P100 GPU accelerators
* 128 GB DDR4 RAM
* 1.8 TB local hard drives
* On-board 10 Gbps Ethernet NIC
* InfiniBand FDR 56 Gbps network card

The node price for Clemson faculty/staff is **$10,000**.

## Purchasing Storage on Palmetto Cluster

Long term storage solutions are available to users seeking
dedicated high performance storage on the cluster.
Owners of storage on Palmetto cluster enjoy the following benefits:

1.  Grant access to purchased storage for other
	Clemson students, faculty or staff
1.	Request access for external collaborators (unaffiliated with Clemson University)
	to purchased storage
1. 	Daily "snapshots" of data for up to 42 days (included in the purchased space).
	Configurable snapshot frequency and period.
1.	Full mirror for system recovery (ITC and main campus)

### Current storage pricing

Storage may be purchased in 1 TB chunks (server shared with other users)
or 150 TB.
Purchase of 150 TB grants the owner access to an isolated storage system,
which may offer better performance.
The current price for both the above options is **$150 per TB**.

The data is stored in the ZFS filesystem format.
Owners of the previous SAMQFS storage
may expand existing storage at the same price.

## Contact information

Individuals interested in purchase of Palmetto hardware
may contact Dustin Atkins at <datkins@clemson.edu>.
