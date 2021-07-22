## Spark with Jupyter Notebook Server

OpenOD has pre-configured jupyter sessions when working with the [spark](https://spark.apache.org/docs/0.9.1/index.html) python library.

When selecting an application from the interavtice app tab select Jupyter with spark
<img src="../../../images/ood/apps/jupyter/jupyter_with_spark.png" style="width:600px">

Once you have arrived at the server configuration page, select the resources (CPU cores, memory, walltime, etc.,) required for your session. Here you can also choose what version of spark you would like to use.

<img src="../../../images/ood/apps/jupyter/jupyter_spark_config.png" style="width:600px">


Once you launch your server you will be taken to your [current list of interactive sessions](https://openod02.palmetto.clemson.edu/pun/sys/dashboard/batch_connect/sessions "current list of interactive sessions"). Your job will be queued until resources are available to handle your request. Once that has happened you will be able to connect to your Jupyter server with a button.
<img src="../../../images/ood/apps/vscode/server_running.png" style="width:800px">


## Launching a Jupyter Server on Palmetto

1. Go to the [OpenOD website](https://openod02.palmetto.clemson.edu/).
2. Click on the **JupyterHub** link. 
3. Log in with your Palmetto user ID and password:
4. Once you are logged in, click on "Interactive apps" on the top navigation bar.

<img src="../../../images/ood/apps//jupyter/jupyter_login.png" style="width:600px">

5. Select the resources (CPU cores, memory, walltime, etc.,) required for your session.

<img src="../../../images/ood/apps/jupyter/jupyter_queue.png" style="width:600px">

6. If the resources you request are available, a notebook server will be started for you.
It is in fact a job submitted to Palmetto under your username. 

<img src="../../../images/ood/apps/jupyter/jupyter_startup.png" style="width:600px">

7. When resources are allocated and the Jupyter server finished launching, your browser
will show the JupyterLab **dashboard**.







