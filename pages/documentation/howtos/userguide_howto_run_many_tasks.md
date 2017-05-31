---
title: How to run several tasks simultaneously (high-throughput jobs)
keywords: [parallel,throughput]
sidebar: documentation_sidebar
permalink: userguide_howto_run_many_tasks.html
---

Often, you may need to perform *high-throughput* computations,
i.e., run many independent tasks concurrently,
with each task operating on a different piece of data.
There are primarily two ways of performing such computations on Palmetto:

## Using job arrays (inefficient method)

You can use a special feature of the PBS scheduling system
called *job arrays* to submit several jobs at once.
An example job script for submitting a job array is
given below:

~~~
#PBS -N quadratic
#PBS -l select=1:ncpus=1:interconnect=1g #PBS -l walltime=00:02:00
#PBS -j oe
#PBS -J 1-8

cd $PBS_O_WORKDIR

./application.x input-${PBS_ARRAY_INDEX}.txt
~~~

The special switch `-J` specifies a range for our jobs (1-8)
Submitting this batch script is equivalent to submitting 8 jobs.
For each of these jobs, the variable
`PBS_ARRAY_INDEX` has a different value in this range
For the first job, `PBS_ARRAY_INDEX` has the value 1,
for the second, it is 2,
and for the last job, it is 8.

To check the status of tasks in job arrays,
you can use the index of each task in your `qstat` command like this:

~~~
qstat 1351352[1].pbs02
qstat 1351352[2].pbs02
~~~

Or you can use the following command to see the status of all
job array tasks you have submitted:

~~~
qstat -Jt -u <username>
~~~

## Using GNU Parallel (preferred method)

Job arrays are fine for small number of independent tasks.
But for larger number of tasks (e.g., hundreds or thousands of tasks),
it is better to use
[GNU Parallel](https://www.gnu.org/software/parallel/).
An example of a batch script which uses GNU Parallel is given below:

~~~
#PBS -N gnu-parallel-example
#PBS -l select=1:ncpus=4:mem=1gb,walltime=00:05:00

module add gnu-parallel

cd $PBS_O_WORKDIR

# process all files in inputs/ directory, 4 at a time:
ls ./inputs/*.txt | parallel -j4 ./application.x

# if the input files are specified in "inputs.txt", we can do:
# parallel -j4 ./application.x < inputs.txt
~~~

In the above batch script,
we use GNU parallel to run our application (`application.x`),
specifying 4 as the number of tasks to run concurrently.
Even if the number of input files was much greater than this,
they are all processed, 4 at a time.

A similar script can be used
when tasks need to be distributed among multiple nodes:

~~~
#PBS -N gnu-parallel-example
#PBS -l select=5:ncpus=4:mem=1gb,walltime=00:05:00

module add gnu-parallel

ls ./inputs/* | parallel --sshloginfile $PBS_NODEFILE -j4 'module add anaconda3/4.2.0; cd /home/username/example; ./application.x {}'
~~~

The above example is similar to the previous one,
but uses multiple (5) chunks of hardware,
and runs 4 tasks per chunk.
When using GNU parallel to run tasks on multiple hardware chunks,
you must also specify the hostname (node) for each chunk.
This information is provided in the file `$PBS_NODEFILE`.

