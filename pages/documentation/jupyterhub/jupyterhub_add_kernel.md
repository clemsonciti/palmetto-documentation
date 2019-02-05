## Adding new kernels

In addition to the default kernels provided (Python, R, and MATLAB), it is possible to create your own kernels. To install kernels for other languages, see the setup instructions for the language [here](https://github.com/ipython/ipython/wiki/IPython-kernels-for-other-languages).

For custom Python kernels, we recommend using [Conda](http://conda.pydata.org/docs/using/index.html) environments, and [ipykernel](http://ipython.readthedocs.io/en/stable/install/kernel_install.html) to generate a kernel from a Conda environment:

~~~
$ conda create --name myenv python=2.7
$ source activate myenv
$ conda install jupyter
$ python -m ipykernel install --user --name python_custom --display-name "My Python"
~~~
