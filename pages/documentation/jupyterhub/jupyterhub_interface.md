---
title: Basic Interface
keywords: [jupyterhub,jupyter,Python,R,MATLAB]
sidebar: documentation_sidebar
permalink: jupyterhub_interface.html
---

## The dashboard

<img src="images/jupyterhub-file-browser.png" style="width:750px">

The dashboard is the primary interface of the notebook server.
You can navigate to the dashboard from any page by clicking on the
Clemson logo on the top left of the page.
By default, the dashboard has three *tabs* named
**Files**, **Running** and **IPython Clusters**. 

### Files and folders

<img src="images/jupyterhub-dashboard-files-tab.png" style="width:750px">

The **Files** tab of the dashboard displays the file explorer,
which lets you interact with files and folders stored on the Palmetto cluster.
Clicking on an item (file or folder) opens it.
Checking the box next to an item lets you perform actions on it,
such as deleting or renaming.

You can create new files or folders by clicking on the "New" button on the top right of the file explorer.

### Terminals

<img src="images/jupyterhub-terminal.png" style="width:750px">

To start a terminal session, click on **New**, and then **Terminal** on the top right of the file explorer.
This opens a terminal running on a compute node of the cluster.
You can have several terminals open at the same time. You can always `ssh` into the login node of the cluster
if you need to submit job requests:

~~~
$ ssh login001
~~~

### Notebooks

<img src="images/jupyterhub-notebook-mode.png" style="width:750px">

Finally, you can create Jupyter notebooks by clicking **New**,
and selecting a notebook *kernel* to use.
Kernels are processes that run interactive code in a
particular programming language and return output to the user.
By default, only Python kernels are provided. But it is very easy to
install additional kernels for langauges such as R and MATLAB.
See [adding new kernels]({{site.baseurl}}/jupyterhub_new_kernels.html).

### Running notebooks and terminals

<img src="images/jupyterhub-dashboard-running-tab.png" style="width:750px">


The **Running** tab of the dashboard shows all running notebooks and terminals.
Simply closing the browser window with a running notebook or terminal
does not shut down that notebook or terminal. They need to be explicitly shut down from this tab.

## Your Notebook Server session

<img src="images/jupyterhub-control-panel.png" style="width:750px">

Your Notebook Server session is really just a PBS job on Palmetto cluster.
Thus, it is valid only for up to the walltime that is selected
when spawning the Notebook Server.
If you exceed the allocated wall time,
your Notebook Server will become unresponsive, and any unsaved work may be lost.
You can stop your Server by clicking on the **Control Panel** button on the top right,
and selecting **Stop My Server**.
You can also log out of your Server by clicking on **Logout**.
Loggig out does not stop the Notebook Server,
which will remain active for the entire allocated wall time.
Logging back in will take you to the running Notebook Server.
