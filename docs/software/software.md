# Software installed on Palmetto

### Modules

A large number of popular software packages
are installed on Palmetto and can be used without
any setup or configuration.
These include:

* Compilers (such as `gcc`, Intel, and PGI)
* Libraries (such as OpenMPI, HDF5, Boost)
* Programming languages (such as Python, MATLAB, R)
* Scientific applications (such as LAMMPS, Paraview, ANSYS)
* Others (e.g., Git, PostgreSQL, Singularity)

These packages are available as modules on Palmetto.
The following commands can be used to inspect, activate
and deactivate modules:

Command |   Purpose
--------|----------------------------------------------------------------------
`module avail` | List all packages available (on current system)
`module add package/version` | Add a package to your current shell environment
`module list` | List packages you have loaded
`module rm package/version` | Remove a currently loaded package
`module purge` | Remove *all* currently loaded packages

See the [Quick Start Guide]({{site.baseurl}}/userguide_quickstart.html)
for more details about modules.

### Licensed software

Many site-licensed software packages are available on Palmetto cluster
(e.g., MATLAB, ANSYS, COMSOL, etc.,).
There are limitations on the number of jobs that can run using these packages.
See [this]({{site.baseurl}}/userguide_howto_check_license_usage.html) section
of the User's Guide on how to check license usage.

Individual-owned or group-owned licensed software can also be run on Palmetto.

### Software with graphical applications

There are two ways to run graphical software on Palmetto:

1. with [X11 tunneling](https://www.palmetto.clemson.edu/palmetto/basic/x11_tunneling/): easy but slow and might not work for graphics-heavy applications;
2. with [VNC](https://www.palmetto.clemson.edu/palmetto/basic/vnc/): fast, more reliable, but trickier to set up.

### Software list

- [abaqus](https://github.com/clemsonciti/palmetto-examples/tree/master/ABAQUS)
- [amber](https://github.com/clemsonciti/palmetto-examples/tree/master/Amber)
- [ansys](https://github.com/clemsonciti/palmetto-examples/tree/master/ANSYS)
- [comsol](https://github.com/clemsonciti/palmetto-examples/tree/master/COMSOL)
- [gephi](https://github.com/clemsonciti/palmetto-examples/tree/master/gephi)
- [gromacs](https://github.com/clemsonciti/palmetto-examples/tree/master/GROMACS)
- [java](https://github.com/clemsonciti/palmetto-examples/tree/master/Java)
- [julia](https://github.com/clemsonciti/palmetto-examples/tree/master/julia)
- [hoodmd](https://github.com/clemsonciti/palmetto-examples/tree/master/hoodmd)
- [lammps](https://github.com/clemsonciti/palmetto-examples/tree/master/LAMMPS)
- [matlab](https://github.com/clemsonciti/palmetto-examples/tree/master/MATLAB)
- [namd](https://github.com/clemsonciti/palmetto-examples/tree/master/namd)
- [paraview](https://github.com/clemsonciti/palmetto-examples/tree/master/Paraview)
- [pytorch](https://github.com/clemsonciti/palmetto-examples/tree/master/PyTorch)
- [rclone](https://github.com/clemsonciti/palmetto-examples/tree/master/rclone)
- [tensorFlow](https://github.com/clemsonciti/palmetto-examples/tree/master/tensorflow)
- [singularity](https://github.com/clemsonciti/palmetto-examples/tree/master/singularity)

