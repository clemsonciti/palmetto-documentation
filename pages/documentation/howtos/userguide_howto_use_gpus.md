---
title: How to use GPUs
keywords: [license]
sidebar: documentation_sidebar
permalink: userguide_howto_use_gpus.html
---

Many packages can take advantage of GPUs to perform computations faster.
To request GPUs, you should specify an additional option `ngpus`
in the resource limits specification in your batch script (batch jobs)
or `qsub` command (interactive jobs).
All GPU nodes on Palmetto have 2 GPUs per node,
so the value for `ngpus` can only be 1 or 2.
In addition, you can also specify
the option `gpu_model` (`m2075`, `m2070q`, `k20`, `k40`, or `p100`)
to request a specific GPU type.

~~~
$ qsub -I  -l select=1:ncpus=1:ngpus=1:gpu_model=k20:mem=2gb,walltime=2:00:00
~~~

To confirm whether your application is using the GPU,
you can SSH into the node that you currently have a job running on,
and run the `nvidia-smi` command, which will list all the processes
running on the node that are using the GPU.
In addition to `/usr/bin/X`, if you see your application in this list,
then it is using the GPU:

~~~
$ ssh node1582
$ nvidia-smi

+-----------------------------------------------------------------------------+
| NVIDIA-SMI 367.44                 Driver Version: 367.44                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla K20m          Off  | 0000:24:00.0     Off |                    0 |
| N/A   52C    P8    19W / 225W |      9MiB /  4742MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  Tesla K20m          Off  | 0000:27:00.0     Off |                    0 |
| N/A   32C    P8    17W / 225W |      9MiB /  4742MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
|    0      5339    G   /usr/bin/X                                       9MiB |
|    1      5339    G   /usr/bin/X                                       9MiB |
+-----------------------------------------------------------------------------+
~~~

