# Running graphical software on Palmetto with VNC
To run graphics-heavy applications that use sophisticated graphical user interfaces (GUI), you can use [VNC](https://en.wikipedia.org/wiki/Virtual_Network_Computing). VNC is a way to connect the graphical output of Palmetto to your local machine. For this purpose, you will need to run *VNC server* on a Palmetto compute node, and to run *VNC client* on your local machine. The instructions for PC and Mac computers are below.

## Running VNC on a PC platform

We really recommend using MobaXTerm, because it comes with built-in VNC client, which makes the process much easier. Use MobaXTerm to log into Palmetto; then, go on a compute node with GPUs:

~~~
qsub -I -l select=1:ncpus=4:mem=50gb:ngpus=2:gpu_model=p100:interconnect=fdr,walltime=8:00:00
~~~

Here, we requested P100 GPUs; you can request any GPU type, based on what's available (see the output of `whatsfree` and `cat /etc/hardware-table`).

Once you get on the compute node, start the VNC server:

~~~
LANG=C && /software/commercial/TurboVNC/bin/vncserver
~~~

If you have never ran it before, it will ask you to create a password. We recommend using the same password as your Palmetto account so it's less confusing. Also, it will give you the information about the node and the port which will be handy for the next step. For example, if it tells you

~~~
>>Desktop 'TurboVNC: node1871.palmetto.clemson.edu:1' started on display node1871.palmetto.clemson.edu:1
~~~
, this means that the node is node1871 and the port is 1. For the next step, we will add 5900 to port number. For our example, the new port number will be 5901.

<img src="../../images/basic/started/turbovnc_setup1.png" style="width:700px">

Don’t close this session! Leave it running.

Now, let’s start the VNC session. Click on Session in top left corner of MobaXTerm, and select VNC. In “Basic VNC settings”, specify the Remote Hostname as the node that you got at the previous step (in our example, it will be `node1871.palmetto.clemson.edu`), as well as the port number (in our case, 5901). Also, click on “Network settings”, select “Connect through SSH gateway” and specify `login.palmetto.clemson.edu` as the gateway SSH server. Specify your Palmetto username (mine is `gyourga`).

<img src="../../images/basic/started/turbovnc_setup2.png" style="width:700px">

Click OK. It will start the VNC session; the first part is two-factor identification:

<img src="../../images/basic/started/turbovnc_setup3.png" style="width:700px">

Don’t enter your password yet! Enter 1 (i.e. number one), which will send you the DUO push. The next step will be entering your password.
This will open a graphical window which runs on Palmetto. It will, most likely, have a Firefox browser running; close it by closing all tabs. You should also see a terminal window running. To make it more user-friendly, type in this window:

~~~
icewm &
~~~

We are now ready to run a graphical application! In order to run it, load the appropriate module, and then type

~~~
vglrun  <program name>
~~~

(For example, type `vglrun comsol`  to run COMSOL, or type `vglrun vmd`  to run VMD, etc.)

You can try running it using four CPUs (or any number of CPUs which does no exceed the **ncpus** that you have specified in your original `qsub`):

~~~
vglrun -np 4 <program name>
~~~

## Running VNC on a Mac platform
Install [TurboVNC for Mac](https://sourceforge.net/projects/turbovnc/files/2.2.3/TurboVNC-2.2.3.dmg/download).

Open a terminal (most likely, it’s in the **Applications** -> **Utilities** folder). Sign into Palmetto, and connect to a compute node (here, we request P100 GPUs; you can request any GPU type which we have available on Palmetto, see the output of `cat /etc/hardware-table` and `whatsfree`):

~~~
qsub -I -l select=1:ncpus=4:mem=50gb:ngpus=2:gpu_model=p100:interconnect=fdr,walltime=8:00:00
~~~

Start TurboVNC server:

~~~
LANG=C && /software/commercial/TurboVNC/bin/vncserver
~~~

If you have never ran it before, it will ask you to create a password. I recommend using the same password as your Palmetto account so it's less confusing. Also, it will give you the information about the node and the port which will be handy for the next step. For example, if it tells you

~~~
>>Desktop 'TurboVNC: node1871.palmetto.clemson.edu:1' started on display node1871.palmetto.clemson.edu:1
~~~

, this means that the node is node1871 and the port is 1. For the next step, we will add 5900 to port number. For our example, the new port number will be 5901.

<img src="../../images/basic/started/turbovnc_setup1.png" style="width:700px">

Open another terminal (don't close the one from the previous step!), and type

~~~
ssh -L 10000:<node>.palmetto.clemson.edu:<port> <username>@login.palmetto.clemson.edu
~~~

For our example, I should type (my username is `gyourga`):

~~~
ssh -L 10000:node1871.palmetto.clemson.edu:5901 gyourga@login.palmetto.clemson.edu
~~~

Finally, on your local computer, open TurboVNC viewer. In "VNC server" field, specify localhost::10000:

<img src="../../images/basic/started/turbovnc_setup4.png" style="width:400px">

Enter password when prompted; this is the password that you have set up earlier when you started the VNC server.
This will open a graphical window which runs on Palmetto. It will, most likely, have a Firefox browser running; close it by closing all tabs. You should also see a terminal window running. To make it more user-friendly, type in this window:

~~~
icewm &
~~~

We are now ready to run a graphical application! In order to run it, load the appropriate module, and then type

~~~
vglrun  <program name>
~~~

(For example, type `vglrun comsol`  to run COMSOL, or type `vglrun vmd`  to run VMD, etc.)

You can try running it using four CPUs (or any number of CPUs which does no exceed the **ncpus** that you have specified in your original `qsub`):

~~~
vglrun -np 4 <program name>
~~~
