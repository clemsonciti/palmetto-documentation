---
title: "Scalable data analysis on Palmetto using Dask"
categories: tutorial 
permalink: dask-on-palmetto.html
tags: [blog]
---

This article describes how to set up and use [https://docs.dask.org/en/latest/](Dask)
on Palmetto Cluster for analyzing big data,
including data that will not fit in memory (RAM).

If you are familiar with Python packages like NumPy and Pandas,
you will find Dask very convenient for data analysis,
as it offers data structures with nearly identical interfaces
to NumPy's `ndarray` and Pandas' DataFrame,
with two major differences:

1. Dask can operate on data that doesn't fit in memory
2. Dask can perform operations in parallel (both shared and distributed-memory)

## Setting up Dask for use on Palmetto and JupyterHub

The script below sets up an environment for conveniently running Dask
either from an interactive IPython Console, Jupyter Notebooks, or in your Python scripts:



### Create a conda environment

```bash
[atrikut@login001 ~]$ module purge
[atrikut@login001 ~]$ module load gcc/5.4.0 openmpi/1.10.3 anaconda3/5.1.0
[atrikut@login001 ~]$ conda create -n dask_env python=3.7
```

### Install necessary packages into the conda environment:

```bash
[atrikut@login001 ~]$ source activate dask_env
(dask_env)[atrikut@login001 ~]$ conda install dask
(dask_env)[atrikut@login001 ~]$ pip install mpi4py ipyparallel
```

### Create a Jupyter Notebook kernel corresponding to the conda environment

```bash
(dask_env)[atrikut@login001 ~]$ python -m ipykernel install --user --name dask_py37 --display-name "Dask (Python 3.7)"
```

That's it - you should now be able to start a notebook server on
[JupyterHub](https://palmetto.clemson.edu/jupyterhub),
and from the drop-down menu,
select "Dask (Python 3.7)" to create a notebook and import dask.

## Dask examples






### Create an IPython profile for parallel computing

This allows you to start a parallel "cluster" of workers that you can connect to from
your Jupyter notebooks:

```bash
(dask_env) [atrikut@login001 ~]$ echo "c.HubFactory.ip = '*'" >> ~/.ipython/profile_MPI/ipcontroller_config.py
```

### Add required module loads to your `.jhubrc`

```bash
(dask_env) [atrikut@login001 ~]$ echo "module load gcc/5.4.0 openmpi/1.10.3" >> ~/.jhubrc
```

### Install the Dask kernel

This step allows you to create and edit notebooks with a 

