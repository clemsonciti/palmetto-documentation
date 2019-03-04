---
title: "Scalable machine learning on Palmetto using Dask"
keywords: [blog, dask, machine-learning, data]
sidebar: documentation_sidebar
permalink: workflows_dask.html
---

Here, we provide an example of a large-scale machine learning workflow on Palmetto cluster
using the Dask Python library.

Check out our [Notebook](https://github.com/clemsonciti/dask-workflows-palmetto/blob/master/training-on-large-datasets.ipynb)
on distributed k-means on Palmetto cluster,
and see below for instructions on running it for yourself.

## What is Dask? 

[Dask](https://docs.dask.org/en/latest) is a Python library
for parallel computing.
Dask makes it easy to use computing resources such as
the Palmetto Cluster
for processing and analyzing data.

Some features of Dask are:

1. [Dask Arrays](https://docs.dask.org/en/latest/array.html):
A parallel, distributed implementation of the NumPy `ndarray` interface
allowing you to work efficiently with very large N-dimensional arrays
commonly used in fields like atmospheric and oceanographic science,
large scale imaging, genomics, algorithms for optimization and statistics, and more.

1. [Dask DataFrame](https://docs.dask.org/en/latest/dataframe.html):
a parallel, distributed implementation of the Pandas DataFrame interface
allowing you to work efficiently with very large tabular data,
and apply common operations such as join, groupby-apply, time series operations, etc.,

1. [Dask-ML](http://ml.dask.org/):
Parallel and distributed algorithms for machine learning
enabling you to train with very large datasets and/or very large models with several hyperparameters.

1. [Dask-image](https://dask-image.readthedocs.io/en/latest/):

  Scalable algorithms for Image Processing of large image data.

## Setting up Dask for use on Palmetto and JupyterHub

1. Start by logging-in to Palmetto, and requesting an interactive job:

   ```
   [atrikut@login001 ~]$ qsub -I -l select=1:ncpus=1,walltime=2:00:00
   qsub (Warning): Interactive jobs will be treated as not rerunnable
   qsub: waiting for job 5518521.pbs02 to start
   qsub: job 5518521.pbs02 ready
   ```
   
1. Clone the [dask-workflows-palmetto](https://github.com/clemsonciti/dask-workflows-palmetto)
   repository (to your home directory or anywhere else):
   
   ```
   [atrikut@node0061 ~]$ git clone https://github.com/clemsonciti/dask-workflows-palmetto
   ```
   
4. Run the `setup-dask.sh` script to install the required packages and scripts
   with the command below.
   Feel free to examine and make changes to this script as required.
   A corresponding script `remove-dask.sh` is provided if you wish to uninstall the components installed:
   
   ```
   [atrikut@node0061 ~]$ cd dask-workflows-palmetto/
   [atrikut@node0061 dask-workflows-palmetto]$ sh setup-dask.sh
   [atrikut@node0061 dask-workflows-palmetto]$ exit # terminate job after setup
   ```
 
## Starting a Dask cluster

After completing the setup above,
you can start a Dask cluster with the commands below.
A Dask cluster is composed of a "scheduler" and 1 or more "workers".

```
[atrikut@login001 ~]$ qsub -I -l select=1:ncpus=20:mem=120gb+4:ncpus=20:mem=120gb,walltime=4:00:00
qsub (Warning): Interactive jobs will be treated as not rerunnable           
qsub: waiting for job 5518518.pbs02 to start                                 
qsub: job 5518518.pbs02 ready                                                
                                     
[atrikut@node1261 ~]$ start-dask-cluster
```

Above, we request 1 chunk with 20 cores and 120gb of memory (the scheduler)
and 4 chunks with 20 cores and 120gb of memory each (the workers).
You can adjust the size of the scheduler and workers depending on your needs
and cluster availability.

The Dask cluster is running for as long as this interactive session is active.
You can connect to this Dask cluster either from an interactive Python console
or from a Jupyter Notebook.

## Example: machine learning with large datasets using Dask-ML

You can now run this [Notebook](https://github.com/clemsonciti/dask-workflows-palmetto/blob/master/training-on-large-datasets.ipynb)
that demonstrates using Dask for simple K-Means clustering of a large dataset:

1. Start by logging-in to Palmetto via JupyterHub (<https://palmetto.clemson.edu/jupyterhub>).
   If you already have a running Notebook server,
   you may need to re-start it.
   
2. Choose any desired settings, note that we will be using the compute resources
   of the Dask cluster started previously, so you may not need a large number of
   cores or RAM for your notebook server.

4. Open and run the Notebook `training-on-large-datasets.ipynb` after navigating
   to the `dask-workflows-palmetto` folder.
   
## Single-machine and distributed modes

Dask can be run on a single node
or across multiple nodes.

To run Dask in single-machine mode,
include the following at the beginning of your Notebook:

```python
from dask.distributed import Client
client = Client()
```

If you wish to use Dask in distributed mode on Palmetto Cluster,
you need to do the following:

1. Start a Dask cluster as shown above
2. In your notebooks, use the following code to start the Dask client:

```python
import getpass
username = getpass.getuser()

from dask.distributed import Client
client = Client(scheduler_file='/home/{}/scheduler.json'.format(username))
client
```

## Further reading

Other examples and worflows can be found at <http://examples.dask.org/>.
You will need to make minor modifications to the notebooks
in order to start the Dask client in single-machine or distirbuted mode (see above section).
