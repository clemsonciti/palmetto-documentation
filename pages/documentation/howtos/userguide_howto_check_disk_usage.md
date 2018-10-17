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

/home/username:          89.5 GB of 100.0 GB; or 334,203 of 1,000,000 files
~~~

The output reports both the number of files and the
total size of files in your home directory.
For example, in the above output,
we see that the total disk usage is 89.5 gigabytes (limit at 100 gb) and the total number of files is 334,203 (limit at 1,000,000 files).

If your disk usage exceeds the size or total number of file limit at **any time**,
you will not be able to save any more data to disk.
