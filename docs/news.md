## April 1st, 2021: Requesting "interconnect=any" and "gpu_model=any" in a qsub

When you request resources on Almetto, you need to specify the type of interconnect (e.g. "fdr", "hdr", "1g"). If you have no preference about the type of interconnect, you can specify `interconnect=any`. Likewise, if you would like to request a compute node with a GPU but you have no preference for a particular GPU model (e.g. "k20", "k40", "p100", "v100"), you can specify `gpu_model=any`. This feature is also implemented in the JupyterHub interface.

For example:

~~~
qsub -I -l select=3:ncpus=4:mem=10gb:interconnect=any:ngpus=1:gpu_model=any,walltime=1:00:00
~~~

Here, we request 3 compute nodes with a GPU, but we don't care which GPU model and which interconnect type they have. We might end up with three nodes having different types of interconnect and different GPU models.

The benefit of this feature is that it increases the pool of nodes that fit your request. For example, if you need 20 nodes with a GPU, but don't care which particular GPU model they have, you won't need to have to wait for 20 nodes with the same GPU model to become available.

Please be aware of the differences in maximum walltime for different interconnect types (the walltime limit for `interconnect=1g` is 336 hours, whereas the walltime limit for `interconnect=fdr` and `interconnect=hdr` is 72 hours). If you request `interconnect=any`, you cannot request for more than 72 hours of walltime. If you need to request more than 72 hours, please use `interconnect=1g`.

Also, **if you are using OpenMPI**, it would be better to stick with `interconnect=1g`, `interconnect=fdr`, and `interconnect=hdr`, rather than use `interconnect=any`. The MPI modules are different on Ethernet nodes (1g) versus Infiniband nodes (fdr and hdr), so there is no guarantee that OpenMPI will run smoothly across nodes with heterogeneous interconnect type.

The old specifications will still work, for example
~~~
qsub -I -l select=3:ncpus=4:mem=10gb:interconnect=1g,walltime=200:00:00
qsub -I -l select=3:ncpus=4:mem=10gb:interconnect=hdr:ngpus=1:gpu_model=v100,walltime=1:00:00
~~~
There is no need to change your old scripts -- they will work as before.
