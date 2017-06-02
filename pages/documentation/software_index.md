---
title: Software on Palmetto Cluster
keywords: [software,programs]
sidebar: documentation_sidebar
permalink: software_index.html
---

## Modules

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

## Licensed software

Many site-licensed software packages are available on Palmetto cluster
(e.g., MATLAB, ANSYS, COMSOL, etc.,).
There are limitations on the number of jobs that can run using these packages.
See [this]({{site.baseurl}}/userguide_howto_check_license_usage.html) section
of the User's Guide on how to check license usage.

Individual-owned or group-owned licensed software can also be run on Palmetto.

## Software with graphical applications

See [this]({{site.baseurl}}/userguide_howto_run_graphical_applications.html) section
of the User's Guide on how to use software with graphical user interface (GUI).

## Installing your own software

See [this]({{site.baseurl}}/userguide_howto_install_software.html) section
of the User's Guide on how to check license usage.
