---
title: "Scalable data analysis on Palmetto using Dask"
keywords: [blog, dask, machine-learning, data]
sidebar: documentation_sidebar
permalink: dask-on-palmetto.html
---

## What is Dask? 

[Dask](https://docs.dask.org/en/latest) is a Python library
for parallel computing.
Dask makes it easy to use computing resources such as
the Palmetto Cluster
for processing and analyzing data.
Some features of Dask are:

1. [A parallel, distributed implementation of the NumPy `ndarray` interface](https://docs.dask.org/en/latest/array.html):
allowing you to work efficiently with very large N-dimensional arrays.
commonly used in fields like atmospheric and oceanographic science,
large scale imaging, genomics, algorithms for optimization and statistics, and more.

1. [A parallel, distributd implementation of Pandas' `DataFrame` interface](https://docs.dask.org/en/latest/dataframe.html):
allowing you to work efficiently with very large tabular data,
and apply common operations such as join, groupby-apply, time series operations, etc.,

1. [Parallel and distributed algorithms for machine learning](http://ml.dask.org/),
enabling you to train with very large datasets and/or very large models with several hyperparameters.

1. [Scalable algorithms for Image Processing](https://dask-image.readthedocs.io/en/latest/) of large image data.

Dask is a complete framework for parallel computing
that scales from a single core on your laptop
to a cluster with 100s of machines.

See examples [here](https:// examples.dask.org/).
See below for instructions on running examples on Palmetto Cluster.

## Setting up Dask for use on Palmetto and JupyterHub

1. Start by logging-in to Palmetto via JupyterHub (<https://palmetto.clemson.edu/jupyterhub>).
   You can choose the default spawner options, but you may want to select a longer walltime (e.g., 1 hour)
2. Open a terminal (click New, and then Terminal)
3. Clone the [dask-workflows-palmetto] repository with the following command:

   ```
   git clone https://github.com/clemsonciti/dask-workflows-palmetto 
   ```
4. Run the `setup-dask.sh` script to install the required packages and scripts
   with the command below.
   Feel free to examine and make changes to this script as required.
   A corresponding script `remove-dask.sh` is provided if you wish to uninstall the components installed:
   
   ```
   cd dask-workflows-palmetto
   sh setup-dask.sh
   ```
 
5. You will need to restart your JupyterHub notebook server before the changes are effected.
   Click "Control Panel" on the top-right, and then "Stop my Server".
   Then start your server again by clicking on "My Server".

## Example
