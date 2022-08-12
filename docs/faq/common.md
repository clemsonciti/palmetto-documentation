# Common problems/issues

## I get the error "undefined symbol: EVP_KDF_ctrl, version OPENSSL_1_1_1b"
This error is caused by a version mismatch between the OpenSSL that is part of the Rocky operating system, and the versions that are required by some software packages. If you get that error, please load these modules:

~~~
module load libssh/0.8.5-gcc/9.5.0 krb5/1.19.3-gcc/9.5.0
~~~

If that fails, load the `anaconda3/2022.05-gcc/9.5.0` module.


## Fine-grained checking of resource availability

Up until recently, whatsfree has been the standard tool to identify available computing resources 
on Palmetto. However, with the additional of compute nodes with large number of cores, 
large amount of memory, and multiple GPUs card, we have seen instances of nodes using only a 
portion of the available cores/memory/GPU but are still regarded as unavailable with whatsfree.

To address this issue and improve utilization on Palmetto, a new tool called `freeres` 
(abbreviation of free resources) has been made available. This tool provides a more fine-grained view 
into individual nodes of a specific phase. For example, at 15:12 November 19, 2021, `whastfree` shows 
that there are no available nodes on phases 18b, 18c, and 19a:

~~~
[lngo@login001 ~]$ whatsfree

C2 CLUSTER (newest nodes with interconnect=HDR except for phase19b)
PHASE 18b  TOTAL =  65  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6148G,     40 cores, 372GB, HDR, 25ge, V100
PHASE 18c  TOTAL =  10  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6148G,     40 cores, 748GB, HDR, 25ge, V100
PHASE 19a  TOTAL =  28  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6248G,     40 cores, 372GB, HDR, 25ge, V100
PHASE 19b  TOTAL =   4  FREE =   1  OFFLINE =   0  TYPE = HPE    XL170   Intel Xeon  6252G,     48 cores, 372GB,      10ge
PHASE 20   TOTAL =  22  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6238R,     56 cores, 372GB, HDR, 25ge, V100S
~~~

However, freeres shows that there are many nodes with significant number of cores/memory available on these phases:

~~~

[lngo@login001 ~]$ freeres phase18b phase18c phase19a                                                                                                   
group file = /software/caci/cluster/phase18b
group file = /software/caci/cluster/phase18c
group file = /software/caci/cluster/phase19a
                 CPU       |       GPU       |   Memory (GB)   |
Node       Avail Used Free | Avail Used Free | Avail Used Free | State
---------------------------------------------------------------------------
node0060    40    12    28     2     2     0   376   372     4   free                                                                                   
node0152    40    26    14     2     2     0   376   173   203   free                                                                                   
node0064    40    12    28     2     2     0   376   372     4   free                                                                                   
node0084    40     8    32     2     2     0   376   372     4   free                                                                                   
node0059    40    30    10     2     2     0   376   173   203   free                                                                                   
node0096    40    32     8     2     2     0   376   184   192   free                                                                                   
node1302    40     3    37     2     0     2   376   372     4   free                                                                                   
node0123    40     8    32     2     2     0   376   372     4   free                                                                                   
node1228    40     8    32     2     2     0   376   372     4   free                                                                                   
node0146    40     8    32     2     2     0   376   372     4   free                                                                                   
node0131    40     8    32     2     2     0   376   372     4   free                                                                                   
node0148    40    37     3     2     2     0   376   304    72   free                                                                                   
node0156    40     8    32     2     2     0   376   372     4   free                                                                                   
node0181    40    33     7     2     2     0   376   358    18   free                                                                                   
node1523    40     8    32     2     2     0   376   372     4   free                                                                                   
node0209    40    32     8     2     2     0   376   258   118   free                                                                                   
node0201    40    12    28     2     2     0   376   372     4   free                                                                                   
node0045    40    36     4     2     2     0   754   240   514   free                                                                                   
node0176    40    30    10     2     2     0   376   244   132   free                                                                                   
node0246    40    12    28     2     2     0   376   372     4   free                                                                                   
node0183    40     8    32     2     2     0   376   372     4   free                                                                                   
node0153    40    32     8     2     2     0   754   270   484   free                                                                                   
node0766    40     8    32     2     2     0   376   372     4   free                                                                                   
node0219    40     8    32     2     2     0   376   372     4   free                                                                                   
node1266    40     8    32     2     2     0   376   372     4   free                                                                                   
node0032    40     8    32     2     2     0   376    44   332   free                                                                                   
node1397    40    32     8     2     2     0   376   264   112   free                                                                                   
node0047    40    16    24     2     2     0   376   372     4   free                                                                                   
node1415    40    36     4     2     1     1   376   270   106   free                                                                                   
node1438    40     8    32     2     2     0   376   372     4   free                                                                                   
node1459    40    26    14     2     2     0   376   174   202   free                                                                                   
node1462    40     8    32     2     2     0   376   372     4   free                                                                                   
node0079    40    32     8     2     2     0   376   128   248   free                                                                                   
node1414    40     8    32     2     2     0   376   372     4   free                                                                                   
node0089    40     8    32     2     2     0   376   372     4   free                                                                                   
node0136    40     8    32     2     2     0   376   372     4   free                                                                                   
node1521    40    32     8     2     1     1   376   214   162   free                                                                                   
node0020    40     8    32     2     2     0   376   372     4   free                                                                                   
node0040    40     8    32     2     2     0   376   372     4   free                                                                                   
node0058    40     8    32     2     2     0   376   372     4   free                                                                                   
node0137    40    32     8     2     2     0   376   128   248   free                                                                                   
node0119    40     8    32     2     2     0   376   372     4   free                                                                                   
node0130    40     8    32     2     2     0   376   372     4   free                                                                                   
checked 103 nodes in 0.42 Seconds

~~~

Taking advantage of freeres, it is possible to request portion of nodes that fit into these available resources. 
For example, the following qsub that specifies `interconnect=25ge` (to imply the usage of these nodes) is allocated in 
less than a minute. 

~~~
[lngo@login001 ~]$ qsub -I -l select=6:ncpus=12:mem=160gb:interconnect=25ge
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 4049542.pbs02 to start
qsub: job 4049542.pbs02 ready

[lngo@node0152 ~]$ 

~~~


## I bought storage on Palmetto; how do I check how much of it I am currently using?

When you buy Palmetto storage, this storage is automatically backed up every day, and the storage space that you have bought is used to store the back-up snapshots. It is very important that your owned storage is less than 90% full, otherwise backups won't work. To check how much space you are using on your bought storage, you can run the script called `checkzfs` from the login node. Let's say I bought 22 Tb of storage, and it's called `mydata`. To check it, I run

~~~
checkzfs mydata
~~~

The output will look something like this:

~~~
DATE:  2021-11-19 14:11:17.878949
============================================
USAGE FOR /zfs/mydata
Purchased: 22.0 TiB
Used: 4.7 TiB
Available: 17.3 TiB

Please leave 10% of the purchased amount for snapshots.
~~~

I can see that my storage uses 4.7 Tb out of 22 Tb, so I am good for now. I need to do it from time to time, to make sure my storage does not exceed 90% (in my case, 19.8 Tb). If it does, `checkzfs` will give me a warning.

## MobaXTerm throws `Authorisation not recognized` error when I try to log into Palmetto

This error looks like this:

~~~
/usr/bin/xauth:  error in locking authority file /home/username/.Xauthority
MoTTY X11 proxy: Authorisation not recognized
~~~

Usually this happens when your home directory is full. Log into Palmetto with another terminal application (for example, [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/)) and free up some space in your home directory. Then, MobaXTerm should work fine.

## I don't see my folder when I do `ls /zfs` 
We recently introduced `autofs` feature that automatically mounts the user directory to `/zfs` file system when they access it, and unmounts it after 5 minutes of inactivity. This feature increases the robustness of our file system, and will greatly decrease the visual clutter (especially important if you are accessing Palmetto through a graphical interface). Due to the automatic mounting, you will not initially see your folder in /zfs, and tab completion won't work. However, the folder is still there. You can `cd` directly into it, and you will not have any issues:

```bash
$ cd /zfs/mygroup
```

After you access it, you will see it when you do `ls /zfs`. If you won't use it for 5 minutes or longer, it will be unmounted, so next time you will have to `cd` into it in order to see it. 

## Program crashes on login node with message `Killed`

When running commands or editing files on the login node, users may
notice that their processes end abruptly with the error message `Killed`.
Processes with names such as `a.out`, `matlab`, etc.,
are automatically killed on the login node because they may consume
excessive computational resources. Unfortunately, this also means that
benign processes, such as editing a file with the word `matlab` as part
of its name could also be killed.

**Solution:** Request an interactive session on a compute node (`qsub -I`),
and then run the application/command.

## Home or scratch directories are sluggish or unresponsive

The `/home` and `/scratch` directories can become slow/unresponsive
when a user (or several users) read/write large amounts of data to
these directories. When this happens, all users are affected as these
filesystems are shared by all nodes of the cluster.

To avoid this issue, keep in mind the following:

1.  **Never** use the `/home` directory as the working directory for
jobs that read/write data. If too many jobs read/write data to the `/home`
directory, it can render the cluster unusable by all users.
Copy any input data to one of the `/scratch` directories and use
that `/scratch` directory as the working directory for jobs.
Periodically move important data back to the `/home` directory.

2.  Try to use `/local_scratch` whenever possible. Unlike `/home`
or the `/scratch` directories, which are shared by all nodes, each
node has its own `/local_scratch` directory. It is much faster to read/write
data to `/local_scratch`, and doing so will not affect other users.
(see example [here])(https://www.palmetto.clemson.edu/palmetto/userguide_howto_choose_right_filesystem.html).

## For MacOS users, problems with logging into Palmetto from the terminal

If you are a Mac user, and try to `ssh` into Palmetto from the terminal, you might get this error message:

```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:Z2WLGvz7vX2t9VPap6ITwS3cBlCafN69FoIm8wmmF6g.
Please contact your system administrator.
Add correct host key in /Users/abcd/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/abcd/.ssh/known_hosts:5
RSA host key for login.palmetto.clemson.edu has changed and you have requested strict checking.
Host key verification failed.
```

This is a common problem, and it's easy to fix. Please find the line "Offending RSA key" in the error message. In the example above, the number of the offending key is 5. This means that we have to remove 5th line from the lists of known SSH hosts so this line could be recreated.

There are several ways to fix the error. The first one is, to type in terminal 

~~~
sed -i '5d' ~/.ssh/known_hosts
~~~

(Please replace the number 5 with the number of the offending RSA key from the error message)

Alternatively, you can type in terminal 

~~~
perl -pi -e 's/\Q$_// if ($. == 5);' ~/.ssh/known_hosts
~~~

(Again, instead of number 5, put the number of the offending RSA key)


## Error when creating conda environment after loading anaconda module
If error occurs, please try the following command:

```bash
`$ export LD_PRELOAD="" `
```

