## Townhall meeting Aug 3rd 2021
After Palmetto maintenance Jul 2021, we hosted the Townhall meeting with all users to talked about changes to Palmetto after maintenance.
The following points were discussed in detail:

1. Introduction Open OnDemand
2. 34 new A100 GPU nodes in phase 27
3. New OMP_NUM_THREADS behavior
4. Renaming of certain Modulefile names
5. Autofs features 
6. Globus transfer file and scratch 1 reinitializing soon
7. New Data transfer node. 

The recording and Q&A can be found in the following link: 
bit.ly/CITI_Townhall_2021

## April 30, 2021: a new version of XQuartz is released, so X11 tunneling now works for Mac OS > 10.14

Good news for Mac users! Recently, there has been an update to [XQuartz](https://www.xquartz.org/), which we can use to set up X11 tunneling on Palmetto in order to run simple graphical applications. The older versions of XQuartz only supported Mac OS up to 10.14 (Mojave); the new version also supports Mac OS 10.15 (Catalina) and Mac OS 11 (Big Sur). We have added [the guide to X11 tunneling](https://www.palmetto.clemson.edu/palmetto/basic/x11_tunneling/) to our website. 


## April 1st, 2021: Requesting "interconnect=any" and "gpu_model=any" in a qsub

When you request resources on Palmetto, you need to specify the type of interconnect (e.g. "fdr", "hdr", "1g"). If you have no preference about the type of interconnect, you can specify `interconnect=any`. Likewise, if you would like to request a compute node with a GPU but you have no preference for a particular GPU model (e.g. "k20", "k40", "p100", "v100"), you can specify `gpu_model=any`. This feature is also implemented in the JupyterHub interface.

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
