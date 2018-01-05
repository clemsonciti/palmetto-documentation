---
title: Configuring your Jupyter Notebook Server
keywords: [jupyterhub,jupyter,Python,R,MATLAB,kernel,conda]
sidebar: documentation_sidebar
permalink: jupyterhub_configuration.html
---

## Your `.jhubrc` file

Before a notebook server is launched,
some modules get loaded by default.
In addition to these default modules,
you can load your own modules,
and export custom environment variables
in a file called `.jhubrc` in your home directory.
All commands in the `.jhubrc` are executed before your notebook server is launched.

For example,
to create a notebook using the MATLAB kernel,
you will need the following lines in your `.jhubrc`:

```bash
module load matlab/2015a zeromq/4.1.5
export MATLAB_EXECUTABLE=$MATLAB/bin/matlab
```

Note that the only commands that will have any effect are `module` and `export` commands. The `.jhubrc` affects only the kernel environments (i.e., the environments of your notebooks), and not the terminal environment.
The terminal environment can be customized using `.bashrc` or `.bash_profile`.

## Installing additional Python/R packages

The default Jupyter environment supports Python and R kernels which are pre-loaded with several common packages for scientific computing and data analysis. It's also easy to install your own packages/libraries for use with these kernels.

We recommend using either pip or [Conda](http://conda.pydata.org/docs/using/index.html) to install
Python packages whenever possible.
You can install most Python package using the terminal with the following commands:

~~~
$ module add anaconda3/4.2.0
$ pip install <package_name> --user
~~~

This will make the package available to the Python 3 kernels.

For R packages, you can define the `R_LIBS` environment variable in your `.jhubrc` to specify where libraries get installed and loaded by notebooks.

