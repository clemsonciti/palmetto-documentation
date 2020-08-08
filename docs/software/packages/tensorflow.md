## Tensorflow

This page explains how to install the [Tensorflow](https://www.tensorflow.org/)
package for use with GPUs on the cluster,
and how to use it from Jupyter Notebook via [JupyterHub](https://www.palmetto.clemson.edu/palmetto/jupyterhub_index.html).

### GPU node

1) Request an interactive session on a GPU node. For example:
~~~
$ qsub -I -l select=1:ncpus=16:mem=20gb:ngpus=1:gpu_model=p100,walltime=3:00:00
~~~

2) Load the Anaconda module:
~~~
$ module load cuda/10.2.89-gcc/8.3.1 cudnn/8.0.0.180-10.2-linux-x64-gcc/8.3.1 anaconda3/2019.10-gcc/8.3.1
~~~

3) Create a conda environment called `tf_env` (or any name you like):
~~~
$ conda create -n tf_gpu_env
~~~

4) Activate the conda environment:
~~~
$ source activate tf_gpu_env
~~~

5) Install Tensorflow with GPU support from the `anaconda` channel:
~~~
$ conda install -c anaconda tensorflow-gpu=2.2.0 python=3.8.3
~~~

This will automatically install some packages that are required for Tensorflow, like SciPy or NumPy. To see the list of installed packages, type

~~~
$ conda list
~~~
If you need additional packages (for example, Pandas), you can type

~~~
$ conda install pandas
~~~

6) You can now run Python and test the install:

~~~
$ python
>>> import tensorflow as tf
~~~

Each time you login, you will first need to load the required modules
and also activate the `tf_env` conda environment before
running Python:

~~~
$ module load cuda/10.2.89-gcc/8.3.1 cudnn/8.0.0.180-10.2-linux-x64-gcc/8.3.1 anaconda3/2019.10-gcc/8.3.1
$ source activate tf_env
~~~

### Installing Tensorflow for non-GPU node

1) Request an interactive session for non-GPU node. For example:

~~~
$ qsub -I -l select=1:ncpus=16:mem=20gb,walltime=3:00:00
~~~

2) Load the required modules

~~~
$ module load cuda/10.2.89-gcc/8.3.1 cudnn/8.0.0.180-10.2-linux-x64-gcc/8.3.1 anaconda3/2019.10-gcc/8.3.1
~~~

3) Create a conda environment called `tf_env_cpu`:

~~~
$ conda create -n tf_env_cpu
~~~

4) Activate the conda environment:

~~~
$ source activate tf_env_cpu
~~~

5) Install Tensorflow from the `anaconda` channel:

~~~
$ conda install -c anaconda tensorflow=2.2.0
~~~

This will automatically install some packages that are required for Tensorflow, like SciPy or NumPy. To see the list of installed packages, type

~~~
$ conda list
~~~
   
If you need additional packages (for example, Pandas), you can type

~~~
$ conda install pandas
~~~

6) You can now run Python and test the install:

~~~
$ python
>>> import tensorflow as tf
~~~

### Add Jupyter kernel:

If you would like to use Tensorflow from Jupyter Notebook on Palmetto via
[JupyterHub](palmetto.clemson.edu/jupyterhub), you need the following additional steps:

1) After you have installed Tensorflow, install Jupyter in the same conda environment:

~~~
$ conda install jupyter
~~~

2) Now, set up a Notebook kernel called "Tensorflow". For Tensorflow with GPU support, do:

~~~
$ python -m ipykernel install --user --name tf_gpu_env --display-name TensorflowGPU
~~~
  
For Tensorflow without GPU support, do:

~~~
$ python -m ipykernel install --user --name tf_env_cpu --display-name Tensorflow
~~~

3) Create/edit the file `.jhubrc` in your home directory:

~~~
$ cd
$ nano .jhubrc
~~~

Add the following two lines to the `.jhubrc` file, then exit.

~~~
module load cuda/10.2.89-gcc/8.3.1 cudnn/8.0.0.180-10.2-linux-x64-gcc/8.3.1 anaconda3/2019.10-gcc/8.3.1
~~~

4) Log into [JupyterHub](https://www.palmetto.clemson.edu/jupyterhub). Make sure you have GPU in your
selection if you want to use the GPU tensorflow kernel

<img src="../../images/software/packages/tensorflow_01.png" style="width:600px">

5) Once your JupyterHub has started, you should see the Tensorflow kernel in your list of kernels
in the Launcher. 

<img src="../../images/software/packages/tensorflow_02.png" style="width:600px">

6) You are now able to launch a notebook using the tensorflow with GPU kernel:

<img src="../../images/software/packages/tensorflow_03.png" style="width:600px">

**Setting up the kernel for non-GPU node is the same as GPU, except using tf_env_cpu**
