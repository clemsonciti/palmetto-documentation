---
title: How to check disk usage
keywords: [space,disk]
sidebar: documentation_sidebar
permalink: userguide_howto_check_disk_usage.html
---

On the login node, you can use the `checkquota` command
to see the disk usage in your home directory:

~~~
$ checkquota

DISK QUOTA (in KB) for HOME DIRECTORY /home/username :
 The default home directory disk quota is 100GB.
 The default home directory file quota is 1 million files.

                                 Online Limits                Total Limits
        Type    ID    In Use     Soft     Hard    In Use     Soft     Hard
/qfs01_home
Files   user 241187    466266  1000000  1001000    466266  1000000  1001000
Blocks  user 241187  47476412 104857600 105906176  47476412 104857600 105906176
Grace period                    1w                          1w

~~~

The output reports both the number of files and the
total size (in Kb) of files in your home directory.
For example, in the above output,
we see that the total disk usage is 47476412 kilobytes
(a kilobyte is 2^10 bytes).

If your disk usage exceeds the "soft limit" (104857600 kilobytes),
you have a grace period of one week to lower disk usage,
but you will still be able to save data to disk
during this grace period.
However, if your disk usage exceeds the "hard limit"
(105906176 kilobytes) at **any time**,
you will not be able to save any more data to disk.

