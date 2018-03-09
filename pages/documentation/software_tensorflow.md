---
title: Tensorflow
keywords: [tensorflow]
sidebar: documentation_sidebar
permalink: software_tensorflow.html
---

# Installing Tensorflow and setting up the corresponding JupyterHub kernel

## Installing Tensorflow

1. Request an interactive session on a GPU node. For example:

   ```
   $ qsub -I -l select=1:ncpus=16:mem=20gb:ngpus=1:gpu_model=k40,walltime=3:00:00
   ```

1. Load the required modules

   ```
   $ module load cuda-toolkit/9.0.176
   $ module load cuDNN/9.0v7
   $ module load anaconda3/5.0.1
   ```

1. Create a conda environment called `tf_env` (or any name you like), with Python 3.6:

   ```
   $ conda create -n tf_env pip python=3.6
   ```

1. Activate the conda environment:

   ```
   $ source activate tf_env
   ```

1. Following the instructions [here](https://www.tensorflow.org/install/install_linux#installing_with_anaconda) for installing Tensorflow:

   ```
   $ pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.5.0-cp36-cp36m-linux_x86_64.whl
   $ conda install matplotlib
   $ conda install scipy
   $ conda install imageio
   ```



## Setting up the kernel for JupyterHub:

If you would like to use Tensorflow from Jupyter Notebook on Palmetto via
[JupyterHub](palmetto.clemson.edu/jupyterhub), you need the following additional steps:

1. Install Jupyter and set up a Notebook kernel called "Tensorflow":

   ```
   $ conda install jupyter
   $ python -m ipykernel install --user --name tf_env --display-name Tensorflow
   ```

1. Create/edit the file `~/.jhubrc`:

   ```
   $ nano .jhubrc
   ```

   Add the following two lines to the `.jhubrc` file, then exit.

   ```
   module load cuda-toolkit/9.0.176
   module load cuDNN/9.0v7
   ```

1. Log into [JupyterHub](https://www.palmetto.clemson.edu/jupyterhub)

   ```
   Suggested settings:
   Num of chunks: 1 (tensorflow will only use one chunk anyway)
   CPU cores: 16
   Memory: 62gb
   Num of GPU: 1 or 2
   Walltime: anything you like
   Queue: workq (do not change)
   ```

1. Click on "New", and select "Tensorflow" (or whatever kernel name you chose in Step 1).
   To test:

   ```
   $ import tensorflow as tf
   ```
