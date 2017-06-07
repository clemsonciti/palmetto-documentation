
# Running a Notebook Server on Palmetto Cluster

Running a Jupyter notebook server on the cluster requires no software or configuration
other than a web browser on your local machine (laptop or workstation).
You will also need an account on the Palmetto cluster.
A new account can be requested by all Clemson students, faculty or staff
[here](http://citi.clemson.edu/new-account/).
The user guide for the Palmetto cluster can be found [here](http://www.palmetto.clemson.edu/pages/userguide.html).

**1.** Start by visiting [https://www.palmetto.clemson.edu/jupyterhub](https://www.palmetto.clemson.edu/jupyterhub).
    

<img src="images/palmetto_login.png" width=600px, style="float:right">
**2.** Log in with your Palmetto user ID and password:

<img src="images/palmetto_start_my_server.png" width=600px style="float:right">
**3.**  Once you are logged in, click on "Start my server" to start a notebook server.



<img src="images/palmetto_spawner_options.png" width=600px style="float:right">
**4.**  You will be taken to the notebook spawner page. Here, you can select the compute resources you will
need access to while working with the notebook server. Resource "chunks" can be thought of as "virtual" nodes. When you request two chunks with 3 CPU cores, 8 GB of RAM, and 1 GPU, you have access to 6 CPU cores, 24 GB of ram and 2 GPUs. However, these resources may reside in two distinct physical nodes of the cluster, or on the same physical node.

<img src="images/palmetto_dashboard.png" width=600px style="float:right">
**5.**  If the resources you request are available, a notebook server will be started for you. This will open up the Notebook **dashboard**, where you will see the files and directories in your "home" directory on the Palmetto cluster. If you have never used the cluster before, your home directory will likely be empty.

**Note:** If you see an error when attempting to spawn a notebook server, or if this page refuses to load, see our Troubleshooting guide for things you can try.
