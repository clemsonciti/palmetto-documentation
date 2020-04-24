---
title: Troubleshooting
keywords: [Troubleshooting]
sidebar: documentation_sidebar
permalink: userguide_troubleshooting.html
---

## Home directory is full

This is a very common situation, and the majority of the problems that Palmetto users experience are related to this. In particular, running out of space in your home directory will make JupyterHub unstable. Also, you will not be able to create any new files in your home directory, which means you cannot create new Anaconda environments and JupyterHub kernels or install new software.

To fix it, [log into Palmetto through a terminal](userguide_basic_usage.html) (**not JupyterHub**). Palmetto users have a limit of 100 Gb on their home directories, so first thing to do is to check the size of your home directory. Once you log in, run this script:
~~~
checkquota
~~~  
We recommend to check your quota on a regular basis, so you have an idea how full your home directory is; this way, running out of space won't come as a surprise.

To see which files and folders take the most space in your home directory, you can use the `du` (["disc usage"](http://www.linfo.org/du.html)) Linux command. Type this sequence:
~~~
cd
du -hs *
~~~
This process might take a while to run. It will give you the size of all the files and folders inside the home directory. To remove them, use the `rm` (["remove"](http://www.linfo.org/rm.html)) command. Folders should be removed with `rm -r`. For example:
~~~
rm my_file.txt
rm -r my_folder
~~~

**NOTE TO JUPYTERHUB USERS:** When you delete a file or a folder in JupyterHub interface, it places it into the Trash folder which is located inside your home directory. Essentially, you are moving a file from one location to another within your home directory, and the total size of the files in your home directory stays the same. To see the contents of your Trash folder, do this:
~~~
cd ~/.local/share/Trash
ls -al
~~~
To remove **all** files in your Trash folder, you can then do this:
~~~
rm -rf *
~~~

## Palmetto account has expired
There are two types of accounts on Palmetto: Research and Educational. Research accounts remain valid as long as your Clemson ID remains active. Educational accounts are valid for one semester and purged from the cluster during the following semester. When the educational account expires, you will not be able to submit any jobs, and, importantly, **you will not be able to start a JupyterHub server**. To fix this, you will need to re-apply for the account as described [here](index.html#obtaining-an-account).

## JupyterHub is not working well
Some common issues with JupyterHub are addressed on the [JupyterHub Troubleshooting](jupyterhub_troubleshooting.html) page.
