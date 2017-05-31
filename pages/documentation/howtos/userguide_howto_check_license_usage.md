---
title: How to check availability of licenses
keywords: [license]
sidebar: documentation_sidebar
permalink: userguide_howto_check_license_usage.html
---

For some of the licensed packages on Palmetto cluster,
you can check the availability (and usage) of licenses
using the `lmstat` command. For example,
to see usage of COMSOL licenses:

~~~
/usr/local/flexlm/lmstat -a -c /usr/local/flexlm/licenses/comsol.dat
~~~

Other packages for which you can check license usage similarly
are listed in the `/usr/local/flexlm/licenses` directory:

~~~
$ ls /usr/local/flexlm/licenses
abaqus.dat    accelrys.dat  ansys.dat     comsol.dat    converge.dat  intel.dat     matlab.dat
~~~
