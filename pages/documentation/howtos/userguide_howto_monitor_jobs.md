---
title: How to monitor running jobs
keywords: [license]
sidebar: documentation_sidebar
permalink: userguide_howto_monitor_jobs.html
---

## Using the qstat Command

Check the status of all of your jobs, or the jobs belonging to a specific user: `qstat -u username`

    [galen@login001 ~]$ qstat -u galen
                                                                Req'd  Req'd   Elap
    Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
    --------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
    956816.pbs01    galen    tiny_lon ubstdv      19540   4  32   36gb 72:00 R 09:26  
    956818.pbs01    galen    tiny_lon ucstdz      31286   4  32   36gb 72:00 R 09:25
    (These 2 jobs are both 4-node, 8-cores-per-node jobs running in the "tiny_long" execution queue, both have been running over 9 hours)
    961935.pbs01    galen    single_q STDIN       32751   1   1    1gb 00:30 R 00:02
    (This is a 1-core interactive job, and it will only last for 30 minutes)
    961936.pbs01    galen    single_q lammps       7116   1   8    2gb 00:20 R 00:00
    (This is a 1-node, 8-cores-per-node job that just started running and it will be running for a maximum walltime of 20 minutes)
    961938.pbs01    galen    tiny_qui namdxa        --    2  16   18gb 01:20 Q   -- 
    (This 2-node, 8-cores-per-node job is still waiting in the queue, it hasn't started running yet)

Check the status and details about a specific queue: `qstat -Qf supabad` (where `supabad` is the name of the 
job queue I want to examine)

For example, here's a snapshot of the Biomolecular Modeling Laboratory's `bioengr` job queue:

    [galen@login001 ~]$ qstat -Qf bioengr
        Queue: bioengr
        queue_type = Execution
        Priority = 200  (Priority > 0 because it's an "owner" queue)
        total_jobs = 25   (Currently, there are 25 jobs in this queue, which includes both queued and running jobs)
        state_count = Transit:0 Queued:0 Held:0 Waiting:0 Running:25 Exiting:0 Begu
        n:0 
        max_running = 640          (Max number of cores available in this queue)
        resources_max.ncpus = 640   (Max number of cores available in this queue)
        resources_max.walltime = 168:00:00   (Max walltime for jobs in this queue)
        acl_group_enable = True   
        acl_groups = bioengr
        default_chunk.mem = 2gb   (Default amount of memory allocated for each requested "chunk" of hardware)
        resources_available.mem = 1280gb  (Total amount of available memory, distributed across all available nodes)
        resources_available.ncpus = 640
        resources_assigned.mem = 854016mb   (Amount of memory currently allocated for jobs)
        resources_assigned.mpiprocs = 552   (Number of MPI processes in use)
        resources_assigned.ncpus = 552     (Number of cores in use)
        resources_assigned.nodect = 69    (Number of nodes in use)
        kill_delay = 30
        enabled = True
        started = True


Check the status and details about a particular job: `qstat -xf 673842` (where `673842` is the job ID 
number of the job I want to examine). The output of the `qstat -xf` command contains a great deal of useful 
information, so let's take a close look at it.  Here, I've used qstat -xf to look at the details of one of my 
jobs:

    [galen@login001 ~]$ qstat -xf 921793

        Job Id: 921793.pbs01
        Job_Name = ubacca
        Job_Owner = galen@login001.palmetto.clemson.edu
        resources_used.cpupercent = 799   (My job is using about 100% of 8 cores on each node)
        resources_used.cput = 335:32:55
        resources_used.mem = 288892kb   (So far, the max memory my job has used is 288,892 / 1,048,576 = 0.276 GB per node)
        resources_used.ncpus = 32
        resources_used.vmem = 1531236kb     (Virtual memory can be thought of as swap space, and most users can ignore this)
        resources_used.walltime = 42:57:31  (My job has been running 42 hrs, 57 min, 31 sec)
        job_state = R
        queue = tiny_long   (My job is running in the "tiny_long" execution queue)
        server = pbs01
        Checkpoint = u   (My job is not being checkpointed)
        ctime = Mon Apr 16 23:04:14 2012   (My job was submitted to the cluster on Monday, April 16th, at 11:04 PM)
        Error_Path = login001.palmetto.clemson.edu:/scratch1/galen/amber/ubacca/ubacca.e921793
        (I'm using  #PBS -j oe  so the contents of this file will be included in my job's output file, see Output_Path below)
        exec_host = node1561/1*8+node1556/0*8+node1556/1*8+node1556/2*8  (The nodes where my job is running)
        exec_vnode = (node1561:ncpus=8:mem=9437184kb)+(node1556:ncpus=8:mem=9437184
        kb)+(node1556:ncpus=8:mem=9437184kb)+(node1556:ncpus=8:mem=9437184kb)
        Hold_Types = n
        Join_Path = oe  (Again, I'm using  #PBS -j oe  so my stderr and stdout will be joined into one file)
        Keep_Files = n
        Mail_Points = ae   (I've requested that the PBS server send me an e-mail if my job is a=aborted or if it e=ends)
        Mail_Users = galen@clemson.edu
        mtime = Mon Apr 16 23:04:45 2012   (Date/time when my job info was last updated -- or the date/time when the job ended)
        Output_Path = login001.palmetto.clemson.edu:/scratch1/galen/amber/ubacca/ubacca.o921793
        (I'm using  #PBS -j oe  so this is where my joined output file will be located after my job ends)   
        Priority = 0   (I submitted this job as a regular "cuuser" user, not using an "owner" queue)
        qtime = Mon Apr 16 23:04:14 2012   (Date/time when my job was accepted into the job queue)
        Rerunable = True   (If my job is preempted by a higher-priority job, my job will be re-started from the beginning)
        Resource_List.mem = 8gb            (In my job script, I requested 2 GB per node)
        Resource_List.mpiprocs = 32   (In my job script, I requested 8 MPI processes per node)
        Resource_List.ncpus = 32     (In my job script, I requested 8 cores per node)
        Resource_List.nodect = 4     (In my job script, I requested 4 "chunks" of hardware)
        Resource_List.place = free:shared   (In my job script, I didn't specify any particular grouping of hardware "chunks")
        Resource_List.select = 4:ncpus=8:mpiprocs=8:mem=9gb   (This is the "select" line from my job script)
        Resource_List.walltime = 72:00:00  (In my job script, I requested a walltime of 72 hours)
        stime = Mon Apr 16 23:04:44 2012   (Date/time when my job started running)
        session_id = 20186
        jobdir = /home/galen   (My job's default staging and execution directory)
        substate = 42
        Variable_List = PBS_O_SYSTEM=Linux,PBS_O_SHELL=/bin/bash,        (PBS environment variaibles available to my job's shell)
        PBS_O_HOME=/home/galen,PBS_O_HOST=login001.palmetto.clemson.edu,
        PBS_O_LOGNAME=galen,PBS_O_WORKDIR=/scratch1/galen/amber/ubacca,
        PBS_O_LANG=en_US.UTF-8,    (PBS_O_WORKDIR = Where my job is actually running, and the location where qsub was executed)
        PBS_O_PATH=/scratch1/galen/protg/mmtsb.v.Jul-31-2009/perl:/usr/lib64/qt
        -3.3/bin:/opt/pbs/default/bin:/opt/gold/bin:/opt/condor/bin:/opt/condor
        /sbin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/opt
        /mx/bin:/home/galen/bin,PBS_O_MAIL=/var/spool/mail/galen,
        PBS_O_QUEUE=workq
        comment = Job run at Mon Apr 16 at 23:04 on (node1561:ncpus=8:mem=9437184kb
        )+(node1556:ncpus=8:mem=9437184kb)+(node1556:ncpus=8:mem=9437184kb)+(no
        de1556:ncpus=8:mem=9437184kb)   (This "comment" section may contain misleading, meaningless info before my job begins running)
        etime = Mon Apr 16 23:04:14 2012   (Date/time when my job became eligible for execution, the needed resources became available)
        eligible_time = 00:00:00   (Time my job has been waiting in the queue -- this is only populated if my has not yet started running)
        Submit_arguments = job.ubacca.rstrt.bash   (This is what followed my qsub command, the arguments I passed to qsub)


This is a lot of output, and is usually more than a user is looking for. An easy way to narrow-down the 
output to just what you might be looking for is to "pipe" the output to grep. For example, to quickly identify 
the "master" node of my allocated nodes:

    [galen@login001 ~]$ qstat -xf 921793 | grep host
    
        exec_host = node1561/1*8+node1556/0*8+node1556/1*8+node1556/2*8

In this line of output, the 1st node listed is the "master" node for my job.


## Monitoring CPU (Processor Core) and Memory Usage

Whenever you have a job running on a cluster node, you are authorized to access that node using SSH. 
One advantage of this is that it allows you to look at the processor core and memory usage reported by that 
specific system (compute node). This can useful for troubleshooting and for other purposes.

Here's an example, let's say I'm running a parallel job named d4modP which uses the CHARMM molecular 
modeling application, and I want to look at the processor core and memory utilization activity on one of the 
nodes where this job is running:

    [galen@login001 ~]$ qstat -u galen | grep d4modP
    995313.pbs01    galen    tiny_sho d4modP      17699   4  32   16gb 18:00 R 00:01

From that output, I can see that my job's job ID number is 995313.

    [galen@login001 ~]$ qstat -xf 995313 | grep host
        exec_host = node0031/0*8+node0102/0*8+node0268/0*8+node0271/0*8

From that output, I can see that my job is running on nodes `node0031`, `node0102`, `node0268`, and `node0271`, 
and I've requested 8 cores on each node. I want to look at the processor core and memory utilziation on one of 
these nodes:

    [galen@login001 ~]$ ssh node0268
    [galen@node0268 ~]$ top -u galen

The top command displays a list of all tasks running on the system (in this case, the compute node I've logged into), 
and this list is updated/refreshed every 2 seconds. I'm using the `-u galen` option to narrow-down the list of tasks 
to just tasks that belong to me:

    Tasks: 233 total,   9 running, 224 sleeping,   0 stopped,   0 zombie
    Cpu(s): 60.2%us,  3.2%sy,  0.0%ni, 35.4%id,  1.2%wa,  0.0%hi,  0.0%si,  0.0%st
    Mem:  12191584k total,  8364844k used,  3826740k free,   374692k buffers
    Swap:  1048568k total,      324k used,  1048244k free,  4359604k cached

      PID USER      PR  NI  VIRT  RES  SHR S %CPU  %MEM    TIME+  COMMAND                                                    
    15214 galen     20   0  762m 130m  19m R 100.3  1.1   2:33.01 charmm                                                    
    15215 galen     20   0  762m 130m  19m R 100.3  1.1   2:32.81 charmm                                                    
    15217 galen     20   0  762m 130m  19m R 100.3  1.1   2:33.14 charmm                                                    
    15216 galen     20   0  762m 130m  19m R  98.4  1.1   2:32.85 charmm                                                     
    15218 galen     20   0  762m 130m  19m R  98.4  1.1   2:33.12 charmm                                                     
    15219 galen     20   0  762m 129m  18m R  98.4  1.1   2:33.07 charmm                                                     
    15212 galen     20   0  762m 129m  18m R  94.4  1.1   2:32.96 charmm                                                     
    15213 galen     20   0  762m 130m  19m R  92.5  1.1   2:33.01 charmm                                                     
    15297 galen     20   0 15080 1232  864 R   2.0  0.0   0:00.01 top                                                        
    15188 galen     20   0 97716 1620  820 S   0.0  0.0   0:00.00 sshd                                                       
    15189 galen     20   0 12884 1072  840 S   0.0  0.0   0:00.03 hydra_pmi_proxy                                            
    15263 galen     20   0 97716 1636  832 S   0.0  0.0   0:00.00 sshd
    15264 galen     20   0  105m 1900 1456 S   0.0  0.0   0:00.00 bash
   
From this output, I can see that I have 8 instances of the CHARMM executable running on this node, 
and they've all been running for about 2 minutes 33 seconds. This is as it should be because I'm running 
CHARMM in parallel, using 8 cores on each node. Each charmm task is using about 100% of the processor core it is 
assigned to, and I'm using very little memory with this job (only about 1.1% of the available memory on this node). 
There are a few other tasks listed there, but these are just "accessory" tasks associated with my job and my current 
Bash shell from logging-in to this node. These additional tasks are barely using any CPU (processor core) or memory 
resources.
  
