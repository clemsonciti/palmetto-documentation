---
title: How to control the placement of jobs
keywords: [node,host,placement]
sidebar: documentation_sidebar
permalink: userguide_howto_control_job_placement.html
---

## Myrinet v/s infiniband nodes

To control the placement of your job on the Myrinet (older)
or Infiniband (newer) part of the cluster,
you can specify the `interconnect=mx` or `interconnect=fdr` option in your
resource limits specification.

~~~
$ qsub -I -l select=1:ncpus=2:mem=2gb:interconnect=mx,walltime=1:00:00
~~~

## Specify the phase

You may specify a particular phase
on which your jobs should run,
using the `phase=` option in your resource limits specification.

~~~
$ qsub -I -l select=1:ncpus=2:mem=2gb:phase=8c,walltime=1:00:00
~~~

## Specify the host

Similarly, you can have your jobs land on a specific
node:

~~~
$ qsub -I -l select=1:ncpus=2:mem=2gb:host=node1744+1:ncpus=2:mem=2gb:node1745,walltime=1:00:00
~~~

Or, in the case of multiple hardware chunks, you can specify
a node for each chunk:

~~~
$ qsub -I -l select=1:ncpus=2:mem=2gb:host=node1105+1:ncpus=2:mem=2gb:host=node1106,walltime=1:00:00
$ cat $PBS_NODEFILE
node1105.palmetto.clemson.edu
node1106.palmetto.clemson.edu
~~~

## Controlling the placement of resource chunks

When requesting multiple chunks of hardware,
you may force each chunk to be on a different node
or on the same node, using `place=scatter` or `place=pack`:

~~~
-l select=2:ncpus=2:mem=15gb,walltime=00:20:00,place=scatter    # force each chunk to be on a different node
-l select=2:ncpus=2:mem=15gb,walltime=00:20:00,place=pack       # force each chunk to be on the same node
~~~
