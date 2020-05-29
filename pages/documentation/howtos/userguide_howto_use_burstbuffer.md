---
title: How to use Burst Buffer
keywords: [burst_buffer]
sidebar: documentation_sidebar
permalink: userguide_howto_use_burstbuffer.html
---

The Burst Buffer is a new high-performance storage system for Palmetto users, consisting of four storage servers running a BeeGFS parallel file system running on flash-based (NVMe) storage along with a rotational disk staging area.  Although the Burst Buffer provides high-throughput and low-latency storage to all applications, its primary benefit is reducing I/O bottlenecks caused by small file operations and non-contiguous shared file access.

NVMe storage is available in two configurations, protected and non-protected. Protected storage mirrors data across NVMe drives on different servers. If an NVMe drive fails, protected data is still available on another server (transparently to the user) and is still accessible if a single storage server goes down. In the non-protected configuration, data is lost if a drive is lost and this data is not accessible if any one of the servers is down.  Reads in the protected configuration perform as well as the non-protected configuration. However, writes in the protected configuration take twice as much time AND space as the non-protected configuration.

Accordingly, for workloads that need the maximum possible write performance AND where results can be easily replicated, non-protected storage is the best choice.  Otherwise, use protected storage.

### Burst Buffer Data Flow

A typical project involving the Burst Buffer, will have the following data flow.

* Copy initial data from ZFS storage to staging.
* For each job run until finished:
    * Copy data from staging to NVMe (fast or fast-buddy)
    * Process data on the compute nodes, interacting with NVMe
    * Copy results back to staging
* Copy final results back from staging to ZFS storage.


<img src="{{site.baseurl}}/images/Burstbuffer_workflow.svg" style="width:750px">
<!-- <img src="images/Burstbuffer_workflow.svg" style="width:750px"> -->

Burst Buffer data flow diagram.

### Resource Requirements
To take advantage of the Burst Buffer’s performance, adequate interconnect and cpus must be requested to ensure a high-performance network connection from the compute node to the Burst Buffer.

You will need to request the following PBS job resources to be able to use the Burst Buffer:
1.	`interconnect={hdr|fdr} `
2.	`ncpus={2 or more}`

The Burst Buffer contains 168TB of NVMe storage, therefore, user’s scripts, input data, and output data cannot exceed:
1.	84 TB, in a protected configuration (NPL=fast-buddy)
2.	168TB, in a non-protected configuration (NPL=fast)
NOTE:  NPL = NVMe Protection Level

### PBS Script Requirements
To automatically copy your data between the staging area and the NVMe drives, use the PBS `stagein` and `stageout` commands.

NOTE:  You should specify a unique output directory in the `stageout` command to avoid writing over your scripts and input files when the working directory is moved from NVMe back to the staging area at the end of the job. Space is not limited in the staging area as it is on the NVMe drives.

Example:

~~~
#PBS -W stagein=/burstbuffer/{NPL}/{userid}/{workdir}@*:/burstbuffer/staging/{userid}/{workdir}/

#PBS -W stageout=/burstbuffer/{NPL}/{userid}/{workdir}/@*:/burstbuffer/staging/{userid}/{outdir}
~~~

where NPL = NVMe Protection Level = `{fast | fast-buddy}` and staging are the hard drives.

In the above example, the `stagein` command copies all files and subdirectories in the `workdir` from the staging area to the NVMe drives, before all other commands in the PBS script are executed.  The `stageout` command copies all files and subdirectories in the `workdir` from the NVMe drives to the `outdir` in the staging area, as the last executed statement in the script.  During the job, you can add or remove files and subdirectories under the `workdir` as is needed by your job.  Then, when the `stageout` command is executed, all files and subdirectories in the `workdir` will be copied to the `outdir`, including any new files and subdirectories the job may have created.


### Requesting Access
1.	Submit a Cherwell ticket asking to use the Burst Buffer.
2.	After approval, move working scripts and data to `/burstbuffer/staging/{userid}/{workdir}`.
3.	Run your job(s).  NOTE:  you will be allowed to run your job(s) several times.
4.	Move results from the output directory to long term storage, when you are happy with your results.  


Tips and FAQs
1.	Why does using the Burst Buffer require permission from the CITI group?

Because manual steps are required to set up the environment.

2.	Why should I use the Burst Buffer?  

Because it is the fastest scratch storage available on Palmetto.

3.	What kind of jobs take advantage of the Burst Buffer?  

Jobs that generate lots of files, or frequently write small amounts of data, or randomly accesses data in a shared dataset across nodes.  Trying it is the best way to know if it helps reduce your job’s walltime.

4.	Is the time that it takes to `stagein` and `stageout` my directory charged against my job’s walltime?  

No, only the execution of your job outside of these statements is charged to the job’s walltime.

5.	Can I `ssh` to the nodes in my job while the `stagein` process is occurring?  

No. Your job status will show that your job is in the running state, but PBS will not allow you to `ssh` to any of the nodes until the `stagein` process has finished.

6.	Does my job hold resources during the `stagein` and `stageout` process?  

Yes.  Your job will be given whatever resources you have requested before the `stagein` process starts and will release those resources after the `stageout` process finishes.

7.	Are my permissions retained when my directory is copied from the staging area to the NVMe?

Yes.  The primary node in your job issues the `cp -rp` command;  `-r` means copy everything recursively, and `-p` means to preserve the ownership, access mode, and timestamps of the original files.

8.	What will `qstat` show me when the `stageout` processing is running?  

`qstat` will show that the job is in the “E” or exit state.  The job will remain in this state until the `stageout` process has completed.

9.	Can I access my files while they are on the NVMe drives? Where can I find them?

Yes.  You can access your files and directories while your job is executing.  You can find them at `/burstbuffer/{NPL}/{userid}`.  After the job finishes, the directory will no longer exist.

10.	Can I access my files and directories in the staging area?  

Yes.  You can access this location at any time from any node (`/burstbuffer/staging/{userid}`) once your Cherwell ticket is approved.

11.	Why do my jobs require `interconnect` to be `hdr` or `fdr`?  

Because the Burst Buffer is connected to the cluster with only the fastest Palmetto networks.  In fact, `hdr` is preferred over `fdr` whenever possible.

12.	What is the difference between protected and unprotected NVMe drives?  

Protected drives mean that if a drive fails, data is not lost, and if a server goes down, the data is still available.  Unprotected means that data is lost if a drive fails, and if a server goes down, the data is not accessible until the server is available again.  

13.	When should I use NVMe drives with protection?  

Most cases should use the NVMe drives with protection. You MUST use NVMe with protection when your job DOES NOT meet the criteria for the unprotected case.  

14.	When should I use NVMe drives without protection?  

If your job needs the full *write* performance capacity of NVMe (read performance is the same for protected and unprotected) OR you need more than 84TB of space.  In both cases, you must be able to reproduce your results!
