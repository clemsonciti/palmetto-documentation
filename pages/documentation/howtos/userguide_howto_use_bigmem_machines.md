---
title: How to use the big-memory machines
keywords: [memory,bigmem]
sidebar: documentation_sidebar
permalink: userguide_howto_use_bigmem_machines.html
---

Phase 0 of the cluster contains
"big memory" machines, each with a large amount of RAM.
To use these machines, you must submit jobs to the
special `bigmem` queue.
For example, in your job script you can include the
`#PBS -q bigmem` directive:

~~~
#PBS -N bigexample
#PBS -l select=1:ncpus=24:mem=494gb
#PBS -q bigmem
~~~

For interactive jobs, just specify `-q bigmem` along with your
`qsub` command:

~~~
$ qsub -I -l select=1:ncpus=24:mem=494gb -q bigmem
~~~
