---
title: MATLAB
keywords: [matlab,MATLAB]
sidebar: documentation_sidebar
permalink: software_matlab.html
---

# MATLAB



## Checking license usage for MATLAB

You can check the availability of MATLAB licenses
using the `lmstat` command:

~~~
/usr/flexlm/lmstat -a -c /usr/flexlm/licenses/matlab.dat 
~~~

## Running the MATLAB graphical interface

To launch the MATLAB graphical interface, you must first
you must [log-in with tunneling enabled]({{site.baseurl}}/pages/userguide/HowTo.html#how-to-run-graphical-software),
and then ask for an interactive session:

~~~
$ qsub -I -X -l select=1:ncpus=1:mem=2gb,walltime=1:00:00
~~~

Once logged-in, you must load one of the MATLAB modules:

~~~
$ module add matlab/2016a
~~~

And then launch the MATLAB program:

~~~
$ matlab
~~~

**Warning**: do not attempt to run MATLAB right after
logging-in (i.e., on the `login001` node).
Always ask for an interactive job first.
MATLAB sessions are automatically killed on the login node.

## Running the MATLAB command line without graphics

To use the MATLAB command-line interface without graphics,
you can additionally use the `-nodisplay` and `-nosplash` options:

~~~
$ matlab -nodisplay -nosplash
~~~

## MATLAB in batch jobs

To use MATLAB in your batch jobs,
you can use the `-r` switch provided by MATLAB,
which lets you **r**un commands specified on the command-line.
For example:

~~~
matlab -nodisplay -nosplash -r myscript
~~~

will run the MATLAB script `myscript.m`.
Thus, an example batch job using MATLAB could have
a batch script as follows:

~~~
#!/bin/bash
#
#PBS -N test_matlab
#PBS -l select=1:ncpus=1:mem=5gb
#PBS -l walltime=1:00:00

module add matlab/2014a

cd $PBS_O_WORKDIR

taskset -c 0-$(($OMP_NUM_THREADS-1)) matlab -nodisplay -nodesktop -nosplash -r test > test_results.txt
~~~

**Note**: MATLAB will sometimes attemps to use all available
CPU cores on the node it is running on.
If you haven't reserved all cores on the node,
your job may be killed if this happens.
To avoid this, you can use the `taskset` utility to
set the "core affinity" (as shown above).
As an example:

~~~
$ taskset 0-2 <application>
~~~

will limit `application` to using 3 CPU cores.
On Palmetto,
the variable `OMP_NUM_THREADS` is automatically set to be the
number of cores requested for a job.
Thus, you can use `0-$((OMP_NUM_THREADS-1))` as shown
in the above batch script to use all the cores you requested.

## Compiling MATLAB code to create an executable

Often, you need to run a large number of MATLAB jobs
concurrently (e.g,m each job operating on different data).
In such cases, you can avoid over-utilizing MATLAB licenses
by *compiling* your MATLAB code into an executable.
This can be done from within the MATLAB command-line as follows:

~~~
>> mcc -R -nodisplay -R -singleCompThread -R -nojvm -m mycode.m
~~~

Note: MATLAB will try to use all the available CPU cores
on the system where it is running, and this presents a problem
when your compiled executable on the cluster where available
cores on a single node might be shared amongst mulitple users.
You can disable this "feature" when you compile your code by
adding the `-R -singleCompThread` option, as shown above.

The above command will produce the executable `mycode`, corresponding
to the M-file `mycode.m`. If you have multiple M-files in your project
and want to create a single excutable, you can use
a command like the following:

~~~
>> mcc -R -nodisplay -R -singleCompThread -R -nojvm -m my_main_code.m myfunction1.m myfunction2.m myfunction3.m
~~~

Once the executable is produced,
ou can run it like any other executable in your batch jobs.
Of course, you'll also need the same `matlab` and
(optional) GCC module loaded for your job's runtime environment.
