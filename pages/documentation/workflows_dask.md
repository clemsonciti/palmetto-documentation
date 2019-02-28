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

1. Start by logging-in to Palmetto via JupyterHub (<http://palmetto.clemson.edu/jupyterhub>).
   You can choose the default spawner options, but you may want to select a longer walltime (e.g., 1 hour)
2. Open a terminal (click New, and then Terminal)
3. Clone the [dask-workflows-palmetto] repository with the following command:

   ```
   $ git clone https://github.com/clemsonciti/dask-workflows-palmetto 
   ```
4. Run the `setup-dask.sh` script to install the required packages and scripts
   with the command below.
   Feel free to examine and make changes to this script as required.
   A corresponding script `remove-dask.sh` is provided if you wish to uninstall the components installed:
   
   ```
   $ cd dask-workflows-palmetto
   $ sh setup-dask.sh
   ```
 
5. You will need to restart your JupyterHub notebook server before the changes are effected.
   Click "Control Panel" on the top-right, and then "Stop my Server".
   Then start your server again by
   clicking on "My Server".
   Run the example below to test your set up.

## Example: machine learning with large datasets using Dask-ML

After completing the setup above,
you can run this [Notebook](https://github.com/clemsonciti/dask-workflows-palmetto/blob/master/training-on-large-datasets.ipynb)
that demonstrates using Dask for simple K-Means clustering of a large dataset:

1. Start by logging-in to Palmetto via JupyterHub (<https://palmetto.clemson.edu/jupyterhub>).
   If you already have a running Notebook server,
   you may need to re-start it.
   
2. Choose the following spawner options:

    * Number of resource chunks: 4
    * CPU cores per chunk: 16
    * Memory per chunk: more than 32gb
    * Walltime: at least 1 hour
    
3. Once your notebook server has started up, start a terminal (New->Terminal), and then run:

   ```
   $ start-dask-cluster
   ```
   
   The cluster takes about a minute to start up.
    
4. Open the Notebook `training-on-large-datasets.ipynb` after navigating
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

1. Start a Dask cluster by running the `start-dask-cluster` script on the Terminal
2. In your notebooks, use the following code to start the Dask client:

```python
import getpass
username = getpass.getuser()

from dask.distributed import Client
client = Client(scheduler_file='/home/{}/dask-scheduler.json'.format(username))
client
```

## Further reading

Other examples and worflows can be found at <http://examples.dask.org/>.
You will need to make minor modifications to the notebooks
in order to start the Dask client in single-machine or distirbuted mode (see above section).
