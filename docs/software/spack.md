# Spack: software package manager

Starting August 2020, Palmetto utilizes [Spack](https://spack.readthedocs.io/en/latest/#) to 
manage the installation and configuration of its research software packages. As a package 
management tool developed by the Lawrence Livermore National Lab (LLNL), Spack helps 
managing the installation and configuration of many research software packages and their 
dependencies in a streamlined and transparent manner.

In the remainder of this documentation, we will show how Palmetto users can also use Spack to 
manage their own software libraries. 

## Setup Spack inside user directory

The following command needs to be run to setup Spack:

```
$ cd
$ mkdir software
$ git clone https://github.com/spack/spack.git
$ source ~/software/spack/share/spack/setup-env.sh
```

You can also append this line to your .bashrc file for it to be automatically loaded as follows:

```
$ echo "source ~/software/spack/share/spack/setup-env.sh" >> ~/.bashrc
$ source ~/.bashrc
```

Copy the default configurations for Spack from Palmetto. You can customize these configurations
as necessary. 

```
$ cp -R /software/spack-src/spack-yaml .spack
```

Modify the `config.yaml` file inside `.spack` with the following content:

```
...

  # This is the path to the root of the Spack install tree.
  install_tree: ~/software/spackages

  # Locations where templates should be found
  template_dirs:
    - /software/spack/share/spack/templates/

  # Default directory layout
  install_path_scheme: "${ARCHITECTURE}/${COMPILERNAME}-${COMPILERVER}/${PACKAGE}-${VERSION}-${HASH}"

  # Locations where different types of modules should be installed.
  module_roots:
    tcl:    ~/software/ModuleFiles/modules
    lmod:   ~/software/ModuleFiles/lmod

...
```

In the file above: 
- `install_tree` specifies where in your home directory you want to install your software. 
- `template_dirs` uses the same template to generate the modulefiles as Palmetto's. You can 
change to your own template if you'd like.
- `install_path_scheme` provides the subdirectory structure within `install_tree`. 
- `tcl` and `lmod` are two different modulefiles specification. Palmetto currently uses `tcl`. 
The generated modulefiles will be placed into the corresponding location. 

## Installing a software package using Spack. 

To view a list of all software supported by Spack, you can run the `spack list` command. At the
moment (July 29, 2020), there are 4483 software packages supported by Spack 
(`ls -l ~/software/spack/var/spack/repos/builtin/packages/ | wc -l`).

Let's assume that you want to install R 3.3.0, which is not currently available on Palmetto (oldest
version is 3.5.3).

First, run the following command to view information about R on Spack

```
$ spack info r
```

<img src="../../images/software/spack/r.png" style="width:600px">

- `Safe versions` list all versions that the Spack team inspected and considered safe/stable to use. You
can see that 3.3.0 is listed among them. 
- `Variants` list different configuration options that are possible. As we will use R on Palmetto, `X11` 
might not be neccessary, but we will want `external-lapack` and `memory_profiling`. 
- The value inside the backet, `[on]` or `[off]`, tell you the default setting of these variants. To turn
on a variant that is defaulted to `[off]`, use `+VARIANT_NAME`. To turn off a variant that is defaulted
to `[on]`, use `~VARIANT_NAME`. 

Before running the installation, we want to run a specification check. This is done by running

```
$ spack spec -I -l r@3.3.0 arch=x86_64 +external-lapack +memory_profiling
```

- The `-I` flag provides information about dependencies. In the display, a gray `-` sign in the first column 
indicates that the dependency is not available and needs to be installed. 
- The `-l` flag provides the unique hash string that identifies each software package installed and managed 
by Spack. 
- If there is any problem with the installation specification (version conflicts, etc), the `spec` call will 
catch the majority of them here. 
  
<img src="../../images/software/spack/r_spec.png" style="width:600px">

If there is no error reported during `spack spec`, we can start the installation process:

```
$ spack install r@3.3.0 arch=x86_64 +external-lapack +memory_profiling
```

After finishing the installation, you will need to add the path to your new module file directory to `$MODULEPATH`:

```
$ export MODULEPATH=$MODULEPATH:~/software/ModuleFiles/modules/linux-centos8-x86_64/
$ module avail
```

You can see your newly install R package:

<img src="../../images/software/spack/r_module.PNG" style="width:600px">

Using `module` allows you to manage the primary installed software, `R 3.3.0`. However, Spack also installed a 
number of dependencies for you. To view all dependencies, run:

```
$ spack find
```

<img src="../../images/software/spack/r_spack_find.PNG" style="width:600px">

These dependencies will be reused by Spack when you install other software packages that require the same 
dependencies. For example, if you run `spack spec -Il r@4.0.0 arch=x86_64`, you will see that many of the 
dependency lines have the green `[+]` sign, indicating that that dependency is already installed. 

<img src="../../images/software/spack/r_dependency.PNG" style="width:600px">


Further information about installing packages using Spack can be found on [Spack's documentation page](https://spack.readthedocs.io/en/latest/basic_usage.html)

