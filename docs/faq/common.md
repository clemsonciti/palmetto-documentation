# Common problems/issues

## I bought storage on Palmetto; how do I check how much of it I am currently using?

When you buy Palmetto storage, this storage is automatically backed up every day, and the storage space that you have bought is used to store the back-up snapshots. It is very important that your owned storage is less than 90% full, otherwise backups won't work. To check how much space you are using on your bought storage, you can run the script called `checkzfs` from the login node. Let's say I bought 8 Tb of storage, and it's called `mydata`. To check it, I run

~~~
checkzfs mydata
~~~

The output will look something like this:

~~~
ZFS QUOTA for /zfs/mydata located on hpczfs05

Max  quota = 8T
Used space = 3.42T
   dataset = 3.42T
   snaps   = 8.71M

(used quota is refreshed every 2 minutes)


ZFS QUOTA for /zfs/mydata located on hpczfsback02

Max  quota = 8T
Used space = 3.65T
   dataset = 3.65T
   snaps   = 36.8M

(used quota is refreshed every 2 minutes)
~~~

The first report (for hpczfs05) is for my primary storage, and the second (for hpczfsback02) is for my backup. I can see that my primary storage uses 3.42 Tb out of 8 Tb, so I am good for now. I need to do it from time to time, to make sure my storage does not exceed 90% (in my case, 7.92 Tb).

## MobaXTerm throws `Authorisation not recognized` error when I try to log into Palmetto

This error looks like this:

~~~
/usr/bin/xauth:  error in locking authority file /home/username/.Xauthority
MoTTY X11 proxy: Authorisation not recognized
~~~

Usually this happens when your home directory is full. Log into Palmetto with another terminal application (for example, [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/)) and free up some space in your home directory. Then, MobaXTerm should work fine.

## I don't see my folder when I do `ls /zfs` or `ls /scratch2`
We recently introduced `autofs` feature that automatically mounts the user directory to `/zfs` and `/scratch2` file system when they access it, and unmounts it after 5 minutes of inactivity. This feature increases the robustness of our file system, and will greatly decrease the visual clutter (especially important if you are accessing Palmetto through a graphical interface). Due to the automatic mounting, you will not initially see your folder in /scratch2 or /zfs, and tab completion won't work. However, the folder is still there. You can `cd` directly into it, and you will not have any issues:

```bash
$ cd /zfs/mygroup
$ cd /scratch2/myusername
```

After you access it, you will see it when you do `ls /zfs` or `ls /scratch2`. If you won't use it for 5 minutes or longer, it will be unmounted, so next time you will have to `cd` into it in order to see it. 

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

## For MacOS user, first time login to Palmetto

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
Offending RSA key in /Users/abcd/.ssh/known_hosts: **1**
RSA host key for login.palmetto.clemson.edu has changed and you have requested strict checking.
Host key verification failed.
```

Resolution: type one of the following command into the terminal before login to Palmetto (Note the number must match with the given key):

```bash
$ sed -i '**1**d' ~/.ssh/known_hosts
$ perl -pi -e 's/\Q$_// if ($. == **1**);' ~/.ssh/known_hosts
```

## Error when creating conda environment after loading anaconda module
If error occurs, please try the following command:

```bash
`$ export LD_PRELOAD="" `
```
