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

## Setup socket proxy access on MacOS/Linux

- Setup the SSH tunnel with port forwarding 
to Palmetto. Open a terminal and run the following command (*this terminal 
must be kept opened for the socket proxy to be active*):

~~~
$ ssh -D 8080 -C -q -N USERNAME@login.palmetto.clemson.edu
~~~

<img src="../../images/advanced/mac_01.png" style="width:600px">


## Setup socket proxy access on Windows

- On the menu button bar of MobaXterm, click Tunneling

<img src="../../images/advanced/win_01.png" style="width:600px">

- Select `New SSH tunnel`, then select `Dynamic port forwarding (SOCKS proxy)`
- Filling the information as follows:
  - `<Forwarded port>`: `8080`
  - `<SSH server>`: `login.palmetto.clemson.edu`
  - `<SSH login>`: USERNAME
  - `<SSH port>`: `22`
- Click `Save`

<img src="../../images/advanced/win_02.png" style="width:600px">

- Click the **play** icon to activate the tunnel. 

<img src="../../images/advanced/win_03.png" style="width:600px">


- You will first be asked for password, that is your Palmetto password. 
- Next, you will be asked for a Duo two-factor login, enter `1` to receive a 
DUO push to your phone. 
- The **play** icon will be blurred, and the **stop** icon will become bolded.
Your tunnel is now activated. You can click on the `X` at the top right corner
to close this windows, and the tunnel will keep running. 

<img src="../../images/advanced/win_04.png" style="width:600px">


## Enable Firefox to view website through the proxy

- This is similar across all 
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

- If Firefox is your primary browser, you will need to reverse the process and 
change back to the default `Use system proxy settings` in your Firefox' 
`Preferences`/`General`/`Network Settings` after you are done. 

## Test proxy

- Login to Palmetto, then get a computer node and launch a Jupyter 
notebook server. 

~~~
$ ssh USERNAME@login.palmetto.clemson.edu
$ qsub -I
$ module load anaconda3/2020.07
$ jupyter notebook --ip=0.0.0.0 --no-browser
~~~

If you are on a Windows machine, use MobaXterm to login to Palmetto as normal. 

<img src="../../images/advanced/test_01.png" style="width:600px">

- Open the browser that has proxy setup and copy the entire notebook link (the one with 
`.palmett.clemson.edu` extension) into the URL bar. 
- Your notebook server should show up, and you can start working as normal. 

<img src="../../images/advanced/test_02.png" style="width:600px">
