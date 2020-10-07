# Setup socket proxy access

There are a number of browser-based applications such as R Studio Server 
for R or Jupyter Server for Jupyter Notebook that could be launched 
directly from the compute nodes. To access these applications, we have 
traditionally used X11-forwarding, meaning to launch an instance of 
Firefox from inside the compute node and display it on the local computer. 
This approach is typically slow. 

In this guide, we will look at how to setup a socket connection with Palmetto 
to enable access to internal web applications inside Palmetto directly on local 
browser programs. While this requires a slightly more elaborate setup than 
X-forwarding, it is also faster and more convenient to use. 

Currently, this guide is designed for Firefox browser. Additional information 
on Chrome, Edge, and Safari will be added. 

- Notation: `USERNAME` means your Palmetto login name. 

## Setup socket proxy access on MacOS

- First, you need to setup the SSH tunnel with port forwarding 
to Palmetto. Open a terminal and run the following command (*this terminal 
must be kept opened for the socket proxy to be active*):

~~~
$ ssh -D 8080 -C -q -N USERNAME@login.palmetto.clemson.edu
~~~

<img src="../../images/advanced/mac_00.png" style="width:300px">



- Next, setup SOCKS proxy for your MacOS
  - Select `Apple` then `System Preferences`
  - Click `Network`
  - Select the network interface you wish to use (i.e. AirPort)
  - Click `Advanced`
  - Click `Proxies` and check the box beside `SOCKS Proxy`
  - Fill in `localhost` and `8080` for the two boxes under
  `SOCKS Proxy Server`. 
  - Make sure that `Use Passive FTP Mode (PASV)` is unchecked. 
  - Click `OK`
  - Click `Apply`

<img src="../../images/advanced/mac_01.png" style="width:600px">


- Download and install Firefox if you don't already have it. 
- Setup Firefox:
  - Select `Firefox` then `Preferences`. 
  - Scroll to the bottomg of `General` and click the `Settings` 
  button in the `Network Settings` section. 
  - Select `Manual Proxy configuration` and fill in `localhost` for 
  `SOCKS Host` and `8080` for `Port`. 
  - Check `SOCKS v5`. 
  - Make sure everything else is unchecked, then click `OK`. 

<img src="../../images/advanced/mac_02.png" style="width:600px">


## Test proxy

- Login to Palmetto, then get a computer node and launch a Jupyter 
notebook server. 

~~~
$ ssh USERNAME@login.palmetto.clemson.edu
$ qsub -I
$ module load anaconda3/2020.07
$ jupyter notebook ip=0.0.0.0 --no-browser
~~~

<img src="../../images/advanced/test_01.png" style="width:600px">

- Open the browser that has proxy setup and copy the entire notebook link (the one with 
`.palmett.clemson.edu` extension) into the URL bar. 
- Your notebook server should show up, and you can start working as normal. 

<img src="../../images/advanced/test_02.png" style="width:600px">
