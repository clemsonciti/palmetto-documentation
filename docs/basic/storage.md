## Palmetto Storage

Today’s scientific research involves increasingly large volumes of increasingly complex data. At the same time,
cpu performance has improved well beyond that of storage media. So, how do we efficiently store and access all of
this data, while keeping hungry cpus sated? Learn the best way to answer this question by reading the following
information on storage technologies and how they can help improve I/O performance on Palmetto.

### Storage Technologies

Palmetto has two types of storage media, hard disk drives and flash drives, each having their own performance characteristics. The hard disk drives have rotating platters and a moving read/write head, the physical mechanisms by which a sector of data is accessed. In contrast, the flash drives require no moving parts to access data, just electrical signals, and so, the flash drives far outperform the hard drives simply due to the nature of the physical media.

Both hard drives and flash drives have their place on Palmetto. Hard drives cost much less per terabyte than flash drives and are very reliable. So, we use hard drives for long term storage to house pools of data, while flash drives are used for computing that require short lived, high I/O throughput.

Contributing to the performance of any storage media is the file system that manages the data. Palmetto engages two types of file systems, traditional and parallel. Both types of file systems take advantage of the underlying storage medium; however, these file systems access data in divergent ways to provide the best possible performance. Traditional file systems, such as `ext4`, `xfs`, and `zfs`, perform best when requests originate from the same machine as the location of storage. Palmetto compute nodes provide a limited amount of this kind of storage, referred to as local storage, that can only be used by an active PBS job. Palmetto also uses a software package called `NFS`, Network File System, which allows compute nodes to access data housed in a traditional file system on a storage server over a network. Parallel file systems, such as `BeeGFS`, `Lustre`, and `GPFS`, perform best when there are many storage servers and a fast network between the compute nodes and the storage servers. These file systems spread data across a set of storage servers, optimizing performance by taking advantage of the processing power of each server and the fast network, while amortizing the time to process a request across the number of storage servers. That is, if a data request takes X amount of time using a traditional file system over a network, then the same request will take X/4 amount of time in a 4 storage server parallel file system. These file systems do not use `NFS` to communicate with the storage servers but have a client process on each compute node. Palmetto is currently using `BeeGFS` as its parallel file system of choice, and all nodes have access to our parallel file systems.

### Palmetto Data Spaces

Palmetto provides three data spaces: home, scratch, and paid storage. Every user has a home directory and access to the scratch file systems. Paid storage is purchased by our research scientists to house their data and can only be accessed by users having the owner’s approval. Each data space is accessible from anywhere in the cluster.

A user’s home data space is accessed by `/home/{userid}` from the command line. Permissions are set to only allow that `{userid}` to access his own directory. Each home directory has a quota of 100GB, is backed up everyday, has hardware protection, and is housed in a traditional ZFS file system on a remote server. The user’s home data space is NOT to be used to process data or as a job’s working directory but is used to store permanent files. Examples:

1. Code repositories, scripts, or notes written by the user.
2. Compiled programs/software
3. Final results from simulations/analyses

Examples of data that should NOT be stored in the home directory:

1. Intermediate data generated during simulations/analyses
2. Read-only data that will be processed to produce some sort of results

Palmetto provides temporary scratch spaces to house intermediate data. These scratch spaces provide varying levels of performance and are accessible from any node. We have three scratch spaces: `/scratch1`, `/fastscratch`, and `/local_scratch`. Every Palmetto user has a directory in `/scratch1` and `/fastscratch` under their userid (`/{filesystem}/{userid}`), and users are not space-limited here like they are with home directories, but these spaces are purged every night of files that have not been accessed in 120 days and 30 days, respectively. So, take care not to leave precious research results or programming assignments in these spaces for very long. Each of these scratch spaces are protected from hardware failures, but the data is NOT backed up. In other words, if a drive fails within a particular file system, that file system can continue to process requests without interruption and no data will be lost. However, if a user deletes a file, then that file is gone without any way to restore it.

Every Palmetto compute node has storage on the node itself and is referred to as `/local_scratch`. This scratch space can only be accessed by a job running on the node. Once the job has ended, the local scratch space for this job is deleted, so your job script must move data from this scratch space to a permanent location to preserve your results. This space is not backed up or guaranteed against hardware failures. You can see the amount of storage by looking at the hardware table from the login node:

```
[login]cat /etc/hardware-table
```

Please note that the hardware table identifies compute nodes by phases and that each phase has a specific disk medium and size to house its `/local_scratch`. This data space is **SHARED** between users’ jobs running on the same node at the same time. If your job requires the entire space, then your job must request the entire node. To request the entire node, specify all the cpu cores for the particular phase in your PBS script. For example:

```
 #PBS select=1:ncpus=16:mem=4gb:phase=8b
```

To access this space in your PBS script, use the `$TMPDIR` environment variable or the location `/local_scratch/pbs.$PBS_JOBID`.
When running a multiple node job and you want to access this local storage on each node, the PBS script must first set up the local storage on each node in the following way:

```
 #ensure that /local_scratch is created on all nodes
 #pbsdsh issues the sleep command on each node assigned
 #to your job, which in turn signals PBS to create the local storage
 #directory on each node before running the sleep command.  Any
 #command may be executed; sleep is just a quick command to
 #process.
   module load gcc/4.8.1 openmpi/1.10.3
   pbsdsh sleep 10
   module purge

#copy data to local_scratch
  for node in `uniq $PBS_NODEFILE`
  do
       ssh $node cp /path/to/remote/dir/{input files} \
                             $TMPDIR/{input files}
 done

# ….other commands

#copy data from local_scratch
 for node in `uniq $PBS_NODEFILE`
 do
   ssh $node cp $TMPDIR/{output files} \
                         /path/to/remote/dir/$node/{output files}
done
```

Paid storage is a data space that is bought by researchers to house their data long term. This space is backed up everyday, protected against hardware failures, and used to store results or software programs. Storage owners access their data space by `/zfs/{group}`, where `{group}` is a Linux group name agreed upon between the owner and the CITI group. Only users who are in the `{group}` are permitted to access an owner’s data space, and only an owner can approve which users are allowed in the `{group}`. These data spaces can be accessed anywhere in the cluster by issuing a `cd` to that space from the command line.

Hardware Grid for Storage

| NAME           | SIZE                  | DISK TYPE    | FILE SYSTEM | NETWORK CONNECTION | BACKUP |
| -------------- | --------------------- | ------------ | ----------- | ------------------ | ------ |
| /home          | 100GB per user        | HDD          | zfs         | ethernet           | yes    |
| /scratch1      | 1933TB                | HDD          | beegfs      | IB, ethernet       | no     |
| /fastscratch   | 168TB                 | NVME         | beegfs      | IB, ethernet       | no     |
| /local_scratch | 99GB - 2.7TB per node | HDD,SSD,NVME | ext4        | internal           | no     |
| /zfs/{group}   | Varies by owner       | HDD          | zfs         | ethernet           | yes    |

HDD = Hard Disk Drive SSD = Solid State Drive NVMe = Non-Volatile Memory Express
IB = InfiniBand GB = gigabyte TB = terabyte

###Performance Guidelines

Generally speaking, `/local_scratch` will always be the fastest file system to use because there is no network involved. However, this space cannot be shared between a group of nodes participating in a job, and the data must be moved to permanent storage before the job completes.

`/scratch1` is a parallel file system that runs atop spinning disk drives and is best suited for workflows issuing sequential, large read or write requests. `/fastscratch` is also a parallel file system but runs atop NVMe flash drives and is best suited for workflows having small, random read or write access patterns. However, `/fastscratch` will also run well with any kind of sequential workload. Either system can handle large numbers of files and directories.

The Palmetto support team encourages you to test your workflows against all three file systems to see which one works best for you.
