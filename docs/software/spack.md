# Spack: software package manager

Starting August 2020, Palmetto utilizes [Spack](https://spack.readthedocs.io/en/latest/#) to 
manage the installation and configuration of its research software packages. As a package 
management tool developed by the Lawrence Livermore National Lab (LLNL), Spack helps 
managing the installation and configuration of many research software packages and their 
dependencies in a streamlined and transparent manner.

In the remainder of this documentation, we will show how Palmetto users can also use Spack to 
manage their own software libraries. 

## Setup Spack inside user directory

In order to install Spack, let's create a folder in our home directory called `software`, enter it, and install Spack from the GitHub repository:

```
$ cd
$ mkdir software
$ cd software
$ git clone https://github.com/spack/spack.git
```

After installation, we will need to run the configuration script to set up the Spack environment: 

```
$ source ~/software/spack/share/spack/setup-env.sh
```

You will have to install Spack only once, but, every time you want to use Spack, you will need to run the configuration script after logging into Palmetto. To avoid doing this, you can run the script from `.bashrc` file which is executed when you log into Palmetto (or go on a compute node). In order to add the script to the `.bashrc` file, you can do this:

```
$ echo "source ~/software/spack/share/spack/setup-env.sh" >> ~/.bashrc
$ source ~/.bashrc
```

Copy the default configurations for Spack from Palmetto. You can customize these configurations
as necessary. 

```
$ cp -R /software/spack-src/spack-yaml.20210524/ ~/.spack
```

Modify the `config.yaml` file inside `~/.spack/linux/` with the following content:

```
...

  # This is the path to the root of the Spack install tree.
  install_tree: ~/software/spackages

  # Locations where templates should be found
  template_dirs:
    - ~/software/spack/share/spack/templates/

  # Default directory layout
  install_path_scheme: "${ARCHITECTURE}/${COMPILERNAME}-${COMPILERVER}/${PACKAGE}-${VERSION}-${HASH}"

  # Locations where different types of modules should be installed.
  module_roots:
    tcl:    ~/software/ModuleFiles/modules
    lmod:   ~/software/ModuleFiles/lmod

...
```

What we are doing is replacing `/software` with `~/software`. `~` refers to your home directory, so `/software` is the directory of all the software on Palmetto cluster (which you can't install into), and `~/software` is the software installed into your home directory.

In the file above: 
- `install_tree` specifies where in your home directory you want to install your software. 
- `template_dirs` uses the same template to generate the modulefiles as Palmetto's. You can 
change to your own template if you'd like.
- `install_path_scheme` provides the subdirectory structure within `install_tree`. 
- `tcl` and `lmod` are two different modulefiles specification. Palmetto currently uses `tcl`. 
The generated modulefiles will be placed into the corresponding location. 

## Installing a software package using Spack. 

To view a list of all software supported by Spack, you can run the `spack list` command. At the
moment (September 14, 2021), there are 4581 software packages supported by Spack 
(`ls -l ~/software/spack/var/spack/repos/builtin/packages/ | wc -l`).

To install software with Spack, **you need to be on a compute node**. So, let's get on one:

```
qsub -I -l select=1:ncpus=10:mem=20gb,walltime=10:00:00
```

If you haven't modified your `.bashrc` file, you will need to run the Spack configuration script:

```
$ source ~/software/spack/share/spack/setup-env.sh
```

Now, we will start the installation of software. As an example, let's install R 3.3.0, which is not currently available on Palmetto (oldest
version is 3.5.3). First, run the following command to view information about R on Spack

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

It is **important** to specify the architecture as `arch=x86_64` if you want your software to run on any Palmetto compute node. We strongly recommend specifying `x86_64`, unless you are an expert and want to install software which will run on some Palmetto nodes but not others.

Installing R might take a while (half an hour or so). If the installation went fine, you should see the module `r` appearing in the `~/software/ModuleFiles/modules/linux-centos8-x86_64/` folder. Try running `module avail` to see if `r` appears in the list of modules:

<img src="../../images/software/spack/r_module.PNG" style="width:600px">

If it does not appear, try adding the path to user-installed modules to `$MODULEPATH`:

```
$ export MODULEPATH=$MODULEPATH:~/software/ModuleFiles/modules/linux-centos8-x86_64/
```

If you still don't see `r` appearing in the output of `module avail`, open the file `~/.spack/linux/modules.yaml` and make sure the package name (in our case, `r`) appears in the `whitelist`.


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

## Uninstalling Spack
If you feel that your Spack installation, or Spack configuration, went wrong, you can uninstall Spack from your home directory:

```
cd
rm -rf .spack
rm -rf software/spack
```

Then, you can re-install Spack from scratch.
