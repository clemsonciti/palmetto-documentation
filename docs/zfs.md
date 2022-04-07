## Purchasing Storage on Palmetto Cluster

Long term storage solutions are available to users seeking dedicated high performance storage on the cluster.
Owners of storage on Palmetto cluster enjoy the following benefits:

1. Grant access to purchased storage for other Clemson students, faculty or staff.
2. Request access for external collaborators (unaffiliated with Clemson University) to purchased storage.
3. Daily "snapshots" of data for up to 7 days (included in the purchased space). Configurable snapshot frequency and period.
4. Full mirror for system recovery (ITC and main campus).

**Please note that the Palmetto cluster should not be used to store sensitive and/or confidential information. Please review Clemson's 
list of data categories [here](https://ccit.clemson.edu/cybersecurity/policy/data-classification/) to make sure that your data belong to 
"public" or "internal use" categories. Data belonging to other categories ("confidential" and "restricted") cannot be stored on Palmetto.**

Storage may be purchased in 1 TB chunks (server shared with other users) or 150 TB. Purchase of 150 TB grants the owner access to an isolated 
storage system, which may offer better performance. 

The purchase is for 4 years (that's the duration of the manufacturer's warranty). After this period, the owner can re-purchase it; 
otherwise, we will clear up the storage using a secure procedure. We can also assist the customer to safely transfer the data from the purchased 
storage to an external location.

To purchase storage, please email `ithelp@clemson.edu` with the subject "Palmetto storage purchase".

## Using purchased Palmetto storage

When a Palmetto user purchases storage, we create a folder in the /zfs directory, with the name that is specified by the user. This storage will be 
accessible to the owner, and to any oter Palmetto user that owner wishes to grant access to (we need an explicit request from the owner in order to 
grant access to another Palmetto user). 

Let's say I bought 5 Tb of storage and want it to be called mystorage. I can access it via

~~~
cd /zfs/mystorage
~~~

Please be aware that, if you do `ls /zfs`, you initially won't see your purchased storage listed (this is an [artefact of our `autofs` mounting 
system](https://www.palmetto.clemson.edu/palmetto/faq/common/#i-dont-see-my-folder-when-i-do-ls-zfs). 
However, you should be able to enter it with `cd`; after you do this, the folder will appear in the `ls /zfs` listing.

To see how much space you are curently using on your purchased storage, use the `checkzfs` command:

~~~
checkzfs mystorage
~~~

## Backups of the purchased storage


Backups are an important feature of our ZFS file system. The contents of the purchased storage are backed up at the end of every day, and stored 
for the period of 7 days (therefore, we can restore a file that was deleted/modified no earlier than 7 days ago). This is not to say that we store 
a physical copy of your data for each of the past 7 days. We create backups using a much more space-efficient process: we record the changes that 
happened during that day (the creation and deletion, and modification of files). This record of changes is called a snapshot. We store the snapshots 
for 7 days, so we can recreate the files within this 7-day period. 

It is very important to realize that the snapshots are stored within the purchased storage. Therefore, if you buy 5 Tb of storage, some part of it will 
be used to store the snapshots. We usually say that people should reserve about 10% of the storage for snapshots; however, this is just a very general rule, 
and the snapshots can easily take more than 10%. The size of the snapshots depends on the amount of activity (creation, deletion, and modification of files). 

Palmetto will send an automatic warning to the owner (and other people who have access to the particular purchased storage) when the amount of used storage 
(data + snapshots) gets over 90% of the purchased amount. Please be aware that, if you delete say 2 Tb of data, the amount of space you are using (the output 
of `checkzfs`) will not immediately decrease by 2 Tb. The reason is that the deletion of data is recorded in snapshots, so the more files you delete, the 
larger the snapshots are. So if you delete 2 Tb, it will take 7 days to see your storage reduce by 2 Tb (because we store the snapshots for 2 days).

In addition, we store a copy of your data as well as the snapshots in a back-up location. To see the size of the data and the snapshots, in both primary 
and backup locations, you can run `checkzfs` with the -a option. Here's a sample output:

~~~
[gyourga@login001 ~]$ checkzfs -a mystorage
DATE:  2022-04-07 14:22:51.780775
============================================
USAGE FOR /zfs/mystorage
Purchased: 5.0 TiB
Used: 4.9 TiB
   Used by primary zfs_data_1/mystorage on hpczfs05: 4.9 TiB
      Used by dataset: 4.7 TiB
      Used by snapshots: 195.5 GiB
   Used by backup zfs_data_1/mystorage on hpczfsback02: 4.9 TiB
      Used by dataset: 4.9 TiB
      Used by snapshots: 68.9 GiB
Available: 70.6 GiB

Please leave 10% of the purchased amount for snapshots.
Warning: Over 90% of zfs quota is used.
~~~

You can see that, on the primary location, the data take 4.7 Tb and the snapshots (for all 7 days combined) take 195.5 Gb. On the backup location, the 
data takes 4.9 Tb ans the snapshots take 68.9 Gb. The contents of the primary and backup locations are the same; the sizes are different because 
these two locations utilize different types of disk drives with different block sizes. In the example above, the data and snapshots take more than 90% 
of the purchased 5 Tb, which means that the owners need to delete some files, otherwise there won't be enough space to store the snapshots and the 
backups will start failing.   
