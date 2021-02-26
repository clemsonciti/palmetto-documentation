## MATLAB



### Checking license usage for MATLAB

You can check the availability of MATLAB licenses
using the `lmstat` command:

~~~
$ /software/USR_LOCAL/flexlm/lmstat -a -c /software/USR_LOCAL/flexlm/licenses/matlab.dat
~~~

### Running the MATLAB graphical interface

To launch the MATLAB graphical interface on a PC/Windows machine, you should use MobaXTerm, and then ask for an interactive session with a -X option:

~~~
$ qsub -I -X -l select=1:ncpus=4:mem=10gb:interconnect=1g,walltime=1:00:00
~~~

Once logged-in, you must load one of the MATLAB modules:

~~~
$ module add matlab/2020a
~~~

And then launch the MATLAB program:

~~~
$ matlab
~~~

Mac users can run Matlab graphical interface through [VNC](https://www.palmetto.clemson.edu/palmetto/basic/vnc/#running-vnc-on-a-mac-platform).

**Warning**: DO NOT attempt to run MATLAB right after
logging-in (i.e., on the `login001` node).
Always ask for an interactive job first.
MATLAB sessions are automatically killed on the login node.

### Running the MATLAB command line without graphics

To use the MATLAB command-line interface without graphics,
you can request a compute node, load the Matlab module, and then use the `-nodisplay` and `-nosplash` options:

~~~
$ matlab -nodisplay -nosplash
~~~

This will work on both Windows and Mac platforms. To quit matlab command-line interface, type:

~~~
$ exit
~~~

### MATLAB in batch jobs

To use MATLAB in your batch jobs,
you can use the `-r` switch provided by MATLAB,
which lets you **r**un commands specified on the command-line.
For example:

~~~
$ matlab -nodisplay -nosplash -r myscript
~~~

will run the MATLAB script `myscript.m`,
Or:

~~~
$ matlab -nodisplay -nosplash < myscript.m > myscript_results.txt
~~~

will run the MATLAB script `myscript.m` and write the output to *myscript_results.txt* file.
Thus, an example batch job using MATLAB could have
a batch script as follows:

~~~
#!/bin/bash
#
#PBS -N test_matlab
#PBS -l select=1:ncpus=4:mem=10gb:interconnect=1g
#PBS -l walltime=1:00:00

module add matlab/2020a

cd $PBS_O_WORKDIR
matlab -nodisplay -nosplash < myscript.m > myscript_results.txt
~~~

### Compiling MATLAB code to create an executable

Often, you need to run a large number of MATLAB jobs
concurrently (e.g,m each job operating on different data).
In such cases, you can avoid over-utilizing MATLAB licenses
by *compiling* your MATLAB code into an executable.
This can be done from within the MATLAB command-line as follows:

~~~
$ matlab -nodisplay -nosplash
>> mcc -R -nodisplay -R -singleCompThread -R -nojvm -m mycode.m
~~~

Note: MATLAB will try to use all the available CPU cores
on the system where it is running, and this presents a problem
when your compiled executable on the cluster where available
cores on a single node might be shared amongst multiple users.
You can disable this "feature" when you compile your code by
adding the `-R -singleCompThread` option, as shown above.

The above command will produce the executable `mycode`, corresponding
to the M-file `mycode.m`. If you have multiple M-files in your project
and want to create a single executable, you can use
a command like the following:

~~~
>> mcc -R -nodisplay -R -singleCompThread -R -nojvm -m my_main_code.m myfunction1.m myfunction2.m myfunction3.m
~~~

Once the executable is produced,
you can run it like any other executable in your batch jobs.
Of course, you'll also need the same `matlab` loaded for your job's runtime environment.
