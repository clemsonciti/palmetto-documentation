#Running graphical software on Palmetto with X11 tunneling

One way to run graphical software on Palmetto is to use [X11 tunneling](https://en.wikipedia.org/wiki/X_Window_System).
It allows you to run a graphical application on the Palmetto compute node, and to control it from your local computer.

Two caveats:

1. running a graphical program through an X11 tunnel might be slow. It will definitely be slower than running it on your local computer.
2. some software, particularly if it's graphics-heavy and uses a lot f OpenGL, might not work with X11 tunneling.
Both of these problems can be solved using [VNC](https://www.palmetto.clemson.edu/palmetto/basic/vnc/) instead of X11 tunneling. VNC is a bit trickier to set up, but it is faster and more powerful.

The process of enabling X11 tunneling is slightly different for Windows, Mac, and Linux users.

## Windows users

For Windows users, we recommend using MobaXTerm to connect to Palmetto. It comes with X11 support, so you don't need to do any extra steps. Just [log into Palmetto as usual](https://www.palmetto.clemson.edu/palmetto/basic/login/#windows). Once you are connected, you can connect to an interactive compute node using the -X switch, for example

~~~
qsub -I -X -l select=1:ncpus=16:mem=60gb:interconnect=fdr,walltime=10:00:00
~~~

Then, you can run your graphical application from the compute node.

## Mac users

Mac users would need to install the program called [XQuartz](https://www.xquartz.org). After installation, double-click on the XQuartz icon to start it (it usually installs into `Applications` -> `Utilities`). Then, in a terminal window, connect to Palmetto with the -Y flag:

~~~
ssh -Y <your username>@login.palmetto.clemson.edu
~~~

Once you are connected, you can connect to an interactive compute node using the -X switch, for example

~~~
qsub -I -X -l select=1:ncpus=16:mem=60gb:interconnect=fdr,walltime=10:00:00
~~~

Then, you can run your graphical application from the compute node.

## Linux Users
No need to install any software. Just log in to Palmetto
from the terminal using the `ssh` command, providing the
additional `-X` switch:

~~~
ssh -X <your username>@login.palmetto.clemson.edu
~~~

Once you are connected, you can connect to an interactive compute node using the -X switch, for example

~~~
qsub -I -X -l select=1:ncpus=16:mem=60gb:interconnect=fdr,walltime=10:00:00
~~~

Then, you can run your graphical application from the compute node.
