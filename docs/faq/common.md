# Common problems/issues

## Account not found in /zfs, /scratch2?
We recently introduced autofs feature that automatically mount the user directory to /zfs and /scratch2 file system.
While there are several benefits such as if user is using GUI software, they only see their storage and not other (to avoid scrolling multiple names). However, it also affects user in such a way that they do not see their directory in /zfs or /scratch2 storage.
The best way to avoid it is to 'cd' directly into their account, for example:
$ cd /zfs/mygroup
$ cd /scratch2/myusername

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

Resolution: type one of the following command into the terminal before login to Palmetto (Note the number must match with the given key):

$ sed -i '**1**d' ~/.ssh/known_hosts

$ perl -pi -e 's/\Q$_// if ($. == **1**);' ~/.ssh/known_hosts

## Error when creating conda environment after loading anaconda module
If error occurs, please try the following command:

`$ export LD_PRELOAD="" `
