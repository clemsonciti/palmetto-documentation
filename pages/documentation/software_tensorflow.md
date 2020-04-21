---
title: Tensorflow
keywords: [tensorflow]
sidebar: documentation_sidebar
permalink: software_tensorflow.html
---

# Installing Tensorflow and setting up the corresponding JupyterHub kernel

This page explains how to install the [Tensorflow](https://www.tensorflow.org/)
package for use with GPUs on the cluster,
and how to use it from Jupyter Notebook via [JupyterHub](https://www.palmetto.clemson.edu/palmetto/jupyterhub_index.html).

## Installing Tensorflow for GPU node

1. Request an interactive session on a GPU node. For example:

   ```
   $ qsub -I -l select=1:ncpus=16:mem=20gb:ngpus=1:gpu_model=p100,walltime=3:00:00
   ```

1. Load the Anaconda module:

   ```
   $ module load cuda-toolkit/9.0.176 cuDNN/9.0v7 anaconda3/5.0.1
   ```

1. Create a conda environment called `tf_env` (or any name you like):

   ```
   $ conda create -n tf_env
   ```

1. Activate the conda environment:

   ```
   $ source activate tf_env
   ```

1. Install Tensorflow with GPU support from the `anaconda` channel:

   ```
   $ conda install -c anaconda tensorflow-gpu=2.0.0
   ```
This will automatically install some packages that are required for Tensorflow, like SciPy or NumPy. To see the list of installed packages, type

   ```
   $ conda list
   ```
If you need additional packages (for example, Pandas), you can type

   ```
   $ conda install pandas
   ```

1. You can now run Python and test the install:

   ```
   $ python

   >>> import tensorflow as tf
   ```

   Each time you login, you will first need to load the required modules
   and also activate the `tf_env` conda environment before
   running Python:

   ```
   $ module load cuda-toolkit/9.0.176 cuDNN/9.0v7 anaconda3/5.0.1
   $ source activate tf_env
   ```




## Installing Tensorflow for non-GPU node

1. Request an interactive session for non-GPU node. For example:

   ```
   $ qsub -I -l select=1:ncpus=16:mem=20gb,walltime=3:00:00
   ```

1. Load the required modules

   ```
   $ module load cuda-toolkit/9.0.176 cuDNN/9.0v7 anaconda3/5.0.1
   ```

1. Create a conda environment called `tf_env_cpu`:

   ```
   $ conda create -n tf_env_cpu
   ```

1. Activate the conda environment:

   ```
   $ source activate tf_env_cpu
   ```

1. Install Tensorflow from the `anaconda` channel:

   ```
   $ conda install -c anaconda tensorflow=2.0.0
   ```

   This will automatically install some packages that are required for Tensorflow, like SciPy or NumPy. To see the list of installed packages, type

   ```
   $ conda list
   ```
   If you need additional packages (for example, Pandas), you can type

   ```
   $ conda install pandas
   ```

1. You can now run Python and test the install:

   ```
   $ python

   >>> import tensorflow as tf
   ```

## Setting up the kernel for JupyterHub:

If you would like to use Tensorflow from Jupyter Notebook on Palmetto via
[JupyterHub](palmetto.clemson.edu/jupyterhub), you need the following additional steps:

1. After you have installed Tensorflow, install Jupyter in the same conda environment:

   ```
   $ conda install jupyter
   ```

1. Now, set up a Notebook kernel called "Tensorflow". For Tensorflow with GPU support, do:

   ```
   $ python -m ipykernel install --user --name tf_env --display-name Tensorflow
   ```
  For Tensorflow without GPU support, do:

   ```
   $ python -m ipykernel install --user --name tf_env_cpu --display-name Tensorflow
   ```

1. Create/edit the file `.jhubrc` in your home directory:

   ```
   $ cd
   $ nano .jhubrc
   ```

   Add the following two lines to the `.jhubrc` file, then exit.

   ```
   module load cuda-toolkit/9.0.176
   module load cuDNN/9.0v7
   ```

1. Log into [JupyterHub](https://www.palmetto.clemson.edu/jupyterhub). Recommended settings:

   ```
   Number of chunks: 1 
   CPU cores: 16
   Memory: 62gb
   Number of GPUs: 1 or 2 (Note: if GPU node is acquired, at least 2 CPU cores are required)
   Queue: workq
   ```

1. Once your JupyterHub has started, you should see the Tensorflow kernel in your list of kernels when you click "New".

### Setting up the kernel for non-GPU node is the same as GPU, except using tf_env_cpu
