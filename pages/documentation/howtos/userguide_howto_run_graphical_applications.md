---
title: How to run graphical applications
keywords: [graphical,graphics,GUI]
sidebar: documentation_sidebar
permalink: userguide_howto_run_graphical_applications.html
---

To run graphical applications on the palmetto cluster,
users will need to "tunnel" or "forward" graphical data
from the cluster to their local machines.
This requires users to be running an "X-server" on their local machines.
Mac and Linux systems typically have an X-server built-in, 
but users of Windows operating systems will need to install
an X-server application to serve this purpose.

![X11 forwarding]({{site.baseurl}}/images/firewall.1.jpg)

## Setup: windows users

Windows users will need the following software to run graphical
applications:

1.  SSH Secure Shell by SSH Communications Security Corporation,
enhanced SSH client with CU 
site license
<http://www.clemson.edu/ccit/software_applications/software/licenses/ssh.html>.

2.  Xming, Windows-based X-server
(<http://www.straightrunning.com/XmingNotes>
and select "Xming" under Public Domain Releases)

When installing Xming,
using all of the default settings is recommended,
but it is not necessary 
to install the built-in PuTTY SSH client:

![Xming setup]({{site.baseurl}}/images/xming.1.jpg)

Xming can be launched from the Windows Start menu.

![Xming setup]({{site.baseurl}}/images/xming.2.jpg)

When you launch Xming,
it will be running in the background,
listening for incoming data.
You can see that it's running by noting the
Xming icon in the system tray:

![Xming setup]({{site.baseurl}}/images/xming.3.jpg)

## Setup: Mac OS X 10.9+ users

Mac OS X users will need to install
XQuartz (http://xquartz.macosforge.org).
XQuartz can be launched from Applications > Utilities.
When you see the XQuartz 'X' icon on your dock,
that indicates that it's running.

## Running an Application on Palmetto with a Tunneled GUI (an example)

As an example,
let's take a look at running COMSOL 4.2a in parallel with a GUI
on Palmetto. 
Tunneling the COMSOL 4.2a GUI will require you to open
2 terminal windows (or 2 SSH Secure Shell windows for Windows users),
both connected to Palmetto with X11 forwarding enabled. 
To accomplish this,
Mac OSX and Linux users need to connect to Palmetto using the
`-X` option in their ssh command:

~~~
ssh -X username@login.palmetto.clemson.edu
~~~

Users of the SSH Secure Shell 
client for Windows will need to enable X11 tunneling by checking the
"Tunnel X11 connections" 
checkbox at **Edit > Settings > Tunneling**,
then save the settings before trying to connect to Palmetto.

In this example,
we'll launch an interactive job for running COMSOL 4.2a using the
COMSOL GUI. 
This interactive job can be started using a
qsub command with the `-X` option, like this:

~~~
qsub -I -X -l select=1:ncpus=4:mpiprocs=4:mem=64gb,walltime=2:00:00
~~~

This qsub command will launch an interactive job
with X11 tunneling
using one "chunk" of cluster hardware consisting of
4 compute cores,
4 MPI processes,
128 GB RAM,
and this job will run for up to 2 hours.

When this interactive job begins running,
you will be logged-in to one of the compute nodes
where you can begin running the COMSOL GUI.

~~~
$ module add comsol/5.2
$ comsol -np4 -tmpdir /local_scratch
~~~

Note:  you are now running COMSOL on Palmetto,
so if you attempt to browse for a file to open, 
you'll only be able to look in the filesystems on Palmetto.
If you need to open a file on your local 
workstation, you must first copy that file to Palmetto
so you can access it there.

Tunneling X11 is not very fast,
so the performance of the GUI will be much slower than 
that of a program running on your local workstation.
It may take approximately 15 seconds 
for the tunneled GUI to be displayed by your local X-server.
Interacting with the tunneled 
GUI (clicking buttons and using menus, etc.) may be slow,
so please consider the amount of data 
moving back-and-forth between your workstation and Palmetto.
The performance may be even worse 
if you're tunneling the GUI to an off-campus location.

![Comsol]({{site.baseurl}}/images/comsol.1.jpg)

