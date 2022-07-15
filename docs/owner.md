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
Owner jobs have a maximum wall time of 336 hours
(2 weeks).

### Current compute hardware and pricing

The configuration of nodes from phase 27 includes:

* 2 x Intel Xeon CLX 6258R (for a total of 56 cores)
* 2 x NVIDIA A100 GPU (40GB mem per GPU)
* 384 GB DDR4 RAM
* On-board 25Gbps Ethernet NIC
* Infiniband HDR-100 Gbps network card

The nodes in phase 27 configuration were available for purchase at subsidized rate for Clemson faculty/staff. At the moment, all the nodes were **sold out**. We will update this page once we have new hardware available for purchasing.  

## Contact information

Individuals interested in purchase of Palmetto hardware
may contact the Advanced Computing and Data Science team to inquire about the availability and subsidized costs for compute nodes  
by emailing ithelp@clemson.edu and including “Palmetto nodes purchase” in the subject line.

## Using purchased Palmetto storage or queue

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
