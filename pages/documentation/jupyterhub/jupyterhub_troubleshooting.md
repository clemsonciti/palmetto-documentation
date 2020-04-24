---
title: JupyterHub Troubleshooting
keywords: [JupyterHub Troubleshooting]
sidebar: documentation_sidebar
permalink: jupyterhub_troubleshooting.html
---

# Troubleshooting

This page lists frequently asked questions and frequently encountered problems, and possible solutions to them.

### I can't log in

First, ensure that you have an account on the Palmetto cluster. You can request an account [here](index.html#obtaining-an-account). If you are repeatedly re-directed to the login page after entering your password correctly, you may need to delete your browser's cookies and try again.

If you have an educational account on Palmetto, it is possible that it has expired. To see whether that's the case, [log into Palmetto through a terminal](userguide_basic_usage.html) (**not JupyterHub**) and then type `qsub -I`. If your educational account has expired, you will see the message `This educational account can no longer submit jobs`. In this case, you will need to [re-apply for a Palmetto account](index.html#obtaining-an-account).

### I can log in, but when I click on "Spawn", nothing happens

This can happen for various reasons:

1. **If the cluster is especially busy**: depending on the resources you request for your Notebook server, it may start immediately, or may be "queued". JupyterHub keeps requests in the queue for up to 5 minutes, after which your request is timed out. You may be successful if you try again after a while.

1. **Due to invalid/excessive resource requests**: for example, asking for a single CPU core and 64 GB of memory is an invalid request, as single core requests are currently limited to 15 GB. It may be useful to consult the [Palmetto user's guide](userguide_palmetto_overview.html) when requesting resources.

1. If you still can't log in, please e-mail <ithelp@clemson.edu> and let us know.

### How do I stop the spawning process?

[Log into Palmetto through a terminal](userguide_basic_usage.html) (**not JupyterHub**) and then type `qstat -u <your Clemson username>`. It will give you a list of jobs that you are trying to run on Palmetto. Find the job with the name `jupyterhub`; in the first column, you will see this job's ID (a 7-digit number). Then, kill it:

~~~
qdel <the 7-digit job ID>
~~~

### I get a "503: Proxy Target Missing" error when trying to spawn a notebook server

Generally, this is OK. Try waiting a few seconds and then refreshing the current page.

### I can't run the MATLAB kernel

Make sure you are requesting at least 14Gb of memory.

### JupyterHub is unstable

The most common cause of this is running out of space in your home directory. Please read [this](userguide_troubleshooting.html#running-out-of-space-in-your-home-directory).

### I need to use a licensed software package inside a JupyterHub kernel

If your notebook needs a licensed package, for example Gurobi, you will need to modify your `.jhubrc` file. Log out of JupyterHub, log into Palmetto through a terminal, and then start the text editor:

~~~
nano ~/.jhubrc
~~~

Once the Nano text editor has started, add this line:

~~~
module load gurobi
~~~

Then, press Ctrl+X to exit and save changes. Gurobi will be activated next time you start a JupyterHub server.
