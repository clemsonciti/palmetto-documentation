## Purchasing Palmetto Compute Resources

Clemson University faculty or staff may purchase immediate access to
Palmetto compute nodes via the "condo model".
Such "owners" enjoy the following benefits:

1. Get immediate access to the purchased compute resources
2. Submit jobs with extended walltime using purchased compute resources (336 hours)
3. Grant access to purchased compute resources for other Clemson students, faculty or staff
4. Request access for external collaborators (unaffiliated with Clemson University) to purchased compute resource

For more information about condominium model and purchasing Palmetto nodes,
including Palmetto nodes on grants, please send an email to <ithelp@clemson.edu> and include the word "Palmetto" in the subject line.

**Please note that the Palmetto cluster should not be used to store sensitive and/or confidential information. Please review Clemson's list of data categories [here](https://ccit.clemson.edu/cybersecurity/policy/data-classification/) to make sure that your data belong to "public" or "internal use" categories. Data belonging to other categories ("confidential" and "restricted") cannot be stored on Palmetto.**

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

The node in phase 27 configuration currently available for purchase at subsidized rate for Clemson faculty/staff is **$7500** for a period of **4 years**:

* 2 x Intel Xeon CLX 6258R (for a total of 56 cores)
* 2 x NVIDIA A100 GPU (40GB mem per GPU)
* 384 GB DDR4 RAM
* On-board 25Gbps Ethernet NIC
* Infiniband HDR-100 Gbps network card

However, due to potential market fluctuations beyond the control of Clemson, we encourage PIs to budget for **$10,000** to ensure all costs are covered.

## Purchasing Storage on Palmetto Cluster

Long term storage solutions are available to users seeking
dedicated high performance storage on the cluster.
Owners of storage on Palmetto cluster enjoy the following benefits:

1.  Grant access to purchased storage for other
	Clemson students, faculty or staff
1.	Request access for external collaborators (unaffiliated with Clemson University)
	to purchased storage
1. 	Daily "snapshots" of data for up to 7 days (included in the purchased space).
	Configurable snapshot frequency and period.
1.	Full mirror for system recovery (ITC and main campus)

**Please note that the Palmetto cluster should not be used to store sensitive and/or confidential information. Please review Clemson's list of data categories [here](https://ccit.clemson.edu/cybersecurity/policy/data-classification/) to make sure that your data belong to "public" or "internal use" categories. Data belonging to other categories ("confidential" and "restricted") cannot be stored on Palmetto.**

### Current storage pricing

Storage may be purchased in 1 TB chunks (server shared with other users)
or 150 TB.
Purchase of 150 TB grants the owner access to an isolated storage system,
which may offer better performance.
The current price for both the above options is **$150 per TB**
for a period of 4 years.

The data is stored in the ZFS filesystem format.
Owners of the previous SAMQFS storage
may expand existing storage at the same price.

## Contact information

Individuals interested in purchase of Palmetto hardware
may contact the Advanced Computing and Data Science team by email ithelp@clemson.edu
and include “Palmetto nodes purchase” (or "Palmetto storage purchase", or if you wish "Palmetto node and storage purchase") in the subject line.

## Using purchased Palmetto storage or queue

Purchased storage is accessible to the owner (and the members of the owner group) as a folder in the `/zfs` directory. For example, if your storage folder is called `my_storage`, you can access it by typing

~~~
cd /zfs/my_storage
~~~

Make sure to [run the `checkzfs` script](https://www.palmetto.clemson.edu/palmetto/faq/common/#i-bought-storage-on-palmetto-how-do-i-check-how-much-of-it-i-am-currently-using) from time to time, to make sure that you don't run over 90% of your disk usage. Otherwise, the file system might start having problems backing up your data.

If you bought priority access to compute nodes on Palmetto, we will create a special queue for you and your group. To submit a job into your queue, you can use the `-q` flag in `qsub`. For example, if your queue is called `my_queue`, an interactive job can be started by

~~~
qsub -I -q my_queue -l select=1:ncpus=2:mem=10gb,walltime=1:00:00
~~~

To submit a batch job into your queue, you should specify the `-q` flag in your PBS script. For example:

~~~
#PBS -N my_job
#PBS -l select=1:ncpus=24:mem=200gb:interconnect=1g,walltime=4:00:00
#PBS -q my_queue
#PBS -m abe
#PBS -M userid@domain.com
#PBS -j oe
~~~

If you are the owner and you would like to add a Palmetto user to your group, please email <ithelp@clemson.edu> with the subject "Adding a user to my Palmetto user group".
