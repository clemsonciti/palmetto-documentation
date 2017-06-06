---
title: Python
keywords: [python,anaconda,conda,pip]
sidebar: documentation_sidebar
permalink: programmers_python.html
---

## Python versions available on Palmetto Cluster

One of the first points of confusion
for many users may be about
the different versions of Python available on the Palmetto cluster.
Currently,
the versions of Python used by the community
are either **2.X.Y** (for example, 2.7.6)
or **3.X.Y** (for example, 3.5.2).
The Palmetto cluster makes many versions of Python available,
and we will begin by enumerating them:

### The default (system) Python

The "default" version of Python on the Palmetto cluster
(also known as the *system* Python),
is **2.6.6**. This is the version of Python that
ships with the operating system running on Palmetto
(currently Scientific Linux release 6.7).

```bash
$ module list
No Modulefiles Currently Loaded.

$ which python
/usr/bin/python

$ python --version
Python 2.6.6
```

In general,
it is highly recommended that users
**not** rely on this version of Python to
run their own applications:

1. Libraries that they use may require other versions
of Python (generally, 2.7 or higher).

2. The system version of Python may change.
Any applications or packages that users
may build using the system Python
will likely not run if this happens.

### Python modules

In addition to the default system Python,
the Palmetto cluster enables users to
load different versions of Python as modules:

```bash
$ module avail python

------------------------- /software/modulefiles -------------------------
python/2.7.6 python/3.3.3 python/3.4
```

Any of these modules can be loaded
to use a different version of Python than 2.6.6:

```bash
$ module add python/3.4

$ python --version
Python 3.4.2

$ which python
/software/python/3.4/bin/python
```

### Anaconda modules

There are also several "Anaconda" modules available on the cluster.

```bash
$ module avail anaconda
anaconda/1.9.1  anaconda/2.3.0  anaconda/2.4.0  anaconda/2.5.0  anaconda/4.0.0  anaconda3/2.5.0 anaconda3/4.0.0

$ module add anaconda3/2.5.0

$ python --version
Python 3.5.2 :: Anaconda 2.5.0 (64-bit)

$ which python
/software/anaconda3/2.5.0/bin/python
```

The `anaconda3` modules contain Python 3,
while the `anaconda` modules contain Python 2.
[Anaconda][anaconda-overview] is a Python "distribution",
which bundles together several packages
that are used in scientific computing and data analysis,
including `numpy`, `scipy`, `matplotlib`, `pandas`, and
several hundreds of others.
Using the Anaconda distribution removes the burden
of manually installing these packages from source.
Once the Anaconda module is loaded,
importing these packages "just works":

```bash
$ module add anaconda3/2.5.0
$ python
Python 3.5.2 |Anaconda 2.5.0 (64-bit)| (default, Jul  2 2016, 17:53:06)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tables       # imports the PyTables package
>>> tables.__file__     # prints where the tables module is loaded from
'/software/anaconda3/2.5.0/lib/python3.5/site-packages/tables/__init__.py'
```
Importantly, Anaconda provides the `conda` package manager,
which will be discussed later in this article.

## Which Python should I use?

In general,
the Python versions provided by the `anaconda` modules
may be a good starting point for most users,
as they also provide several widely-used packages.
However, if you want to build and maintain your own versions
of Python packages,
you may want to use the `python` modules.

Later in this article,
we will also see another option:
using `conda` to easily install *any* version of Python,
and easily install packages for this version.

## Installing Packages

Although the `anaconda` modules already provide several of the
widely used packages,
you may need to install other packages or different versions
of the available packages for your own project.
This can sometimes be a challenge,
because some packages may require other packages (i.e., they have dependencies),
and because users do not have root (administrative) privileges on the cluster.
Most packages and their dependencies can be installed in one of the following ways:

1. Using `pip`
2. Downloading and building the package and its dependencies yourself
3. Using the `conda` package manager

Several packages will provide the option
to install in more than one way.
For example, see the [installation instructions
for the `mpi4py` package][mpi4py-install],
which can be installed either using `pip`,
or by downloading and building from source.

It is not necessary that you install *all* packages
using `pip`,
or build *all* packages yourself.
Your setup can contain several packages,
and each one may be installed in any of the above ways.

### Using `pip` to install packages and dependencies

`pip` is a program that installs Python packages,
and automatically installs any other Python packages
that are dependencies:

```bash
$ pip install package-name --user
```

The `--user` switch ensures that the package is installed
into your home directory, and not into the `/usr/local` directory.
This will only install the package
for the currently running version of Python.

Unfortunately,
`pip` is generally not well suited for installing scientific software packages
like `numpy` or `matplotlib` (see [here](https://packaging.python.org/science/) for explanation).

### Building the package yourself

When packages cannot be installed via `pip`
or when you want a package to be built in a specific way,
e.g., linked against specific libraries,
you may need to download and build the package for yourself.
The instructions for building a package are generally
a part of the package's documentation.
For example, see the instructions to manually install the `mpi4py` package
[here][mpi4py-install]. Importantly, notice the last step:

```bash
python setup.py install --user
```

The `--user` switch ensures that the package is installed
to your home directory, and not a directory like `/usr/local`.
Even when you manually build a package,
it is only compatible and available for the specific
version of Python loaded while building it.

When building packages yourself,
you will generally have to manage dependencies
yourself, i.e., they will not automatically be installed.

### The `conda` package manager

`conda` is a package manager similar to `pip`,
but with two major improvements over `pip`:

1. `conda` also installs non-Python packages and dependencies.
For example, the `numpy` package has dependencies such as `ATLAS`, `gfortran`, etc.,
and `pip` is not able to install these for you.
`conda` will automatically install such dependencies.

2. `conda` also lets users create and manage "environments".
A `conda` environment is a directory
containing a specific version of Python
and a collection of packages for this version of Python.

`conda` is available as part of any of the `anconda` modules.

#### Creating an environment

You can create a `conda` environment using the
`conda create` command, and specify
what version of Python to install in this environment:

```bash
$ module add anaconda3/2.5.0
$ conda create -n my_env python=3.5
```

Once an environment is created,
you can "enter", or "activate" it using the following command:

```bash
$ source activate my_env
```

You will now be running the environment's Python:

```bash
(my_env) $ python --version
Python 3.5.2 :: Continuum Analytics, Inc.

(my_env) $ which python
~/.conda/envs/my_env/bin/python
```

You can "exit" or "deactivate" the environment using the following command:

```bash
(my_env) $ source deactivate
$ 
```

#### Installing packages in an environment

A newly created `conda` environment
contains a few core packages,
but in general, you will have to install
whatever packages you need inside this environment.
Any packages you install will be available only
within the environment.

The `conda install` command
can be used to download many
packages popularly used in scientific computing and data analysis.
A full list of packages is maintained [here][anaconda-pkg-list].
For example:

```bash
(my_env) $ conda install numpy
```

You can even install a specific version:

```bash
(my_env) $ conda install numpy=1.11
```

You can also use `pip` to install packages;
but you don't need the `--user` switch;
`pip` will automatically install into the environment's directory.

```bash
(my_env) $ pip install package-name
```

Lastly, you can build packages manually,
but you don't need the `--user` switch
when running `python setup.py`.
`pip` will automatically install into the environment's directory.

#### Cloning an existing environment

When you load any of the `anaconda` modules,
the default `conda` environment is the `root` environment.
Although you can use all the packages that are part
of this environment,
you do not have permissions to install any packages
into this environment.

You can however, create your own personal "clone"
of the `root` environment, as follows:

```bash
$ conda create -n my_root --clone=/software/anaconda3/2.5.0
```

#### `conda` documentation

For more information about the `conda` package manager,
and for more advanced package management,
see the official `conda` documentation [here][conda-docs].

## Using `conda` environments to manage multiple Python projects

Suppose you are working on two different projects
which have the following requirements:

<img src="{{site.baseurl}}/images/python-envs.png" style="width:600px">

The first project requires
Python 2.7 and a set of Python packages such as `numpy`.
The second package requires Python 3.4,
and a different set of Python packages.
The recommended method is to
set up a separate `conda` environment for each project,
and install the specific versions of packages required.
Within each environment,
you can use `pip` or `conda` to install the packages,
or build them yourself from source.

[mpi4py-install]: https://mpi4py.scipy.org/docs/usrman/install.html
[python-packaging-science]: https://packaging.python.org/science/
[anaconda-overview]: https://www.continuum.io/anaconda-overview
[anaconda-pkg-list]: https://docs.continuum.io/anaconda/pkg-docs
[conda-docs]: http://conda.pydata.org/docs/
