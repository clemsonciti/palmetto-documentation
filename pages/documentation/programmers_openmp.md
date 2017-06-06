---
title: OpenMP
keywords: [openmp, parallel, multicore]
sidebar: documentation_sidebar
permalink: programmers_openmp.html
---

Open Multi-Processing (OpenMP) is a programming interface designed for taking advantage
of more than one processor (or core) in a single node. OpenMP consists of a collection of 
compiler directives. All compiler suites available on Palmetto support OpenMP standard. 

The simple "Hello world" program can be augmented with OpenMP directives to allow multiprocessing

{% highlight cpp %}
#include <stdio.h>
 
int main(){
#pragma omp parallel
  printf("Hello, world.\n");
  return 0;
}
{% endhighlight %}

To enable the OpenMP directives one has to tell the compiler to do so using a compiler specific
command line option. The file above `hello_omp.c` can be compiled on Palmetto using the following
commands

Compiler | Module        | Command
-------- | ------------- | --------------------
Intel    | `intel/13.0`  | `icc -openmp hello_omp.c -o hello.x`
GCC      | `gcc/4.8.1`   | `gcc -fopenmp hello_omp.c -o hello.x`
PGI      | `pgi/14.2`    | `pgcc -mp hello_omp.c -o hello.x`  

The number of threads used in the program compiled with OpenMP directives is controlled using 
the environmental variable `OMP_NUM_THREADS`. The value of `OMP_NUM_THREADS` is automatically set
to the number of requested processors per node. For example

    $ qsub -I -l select=1:ncpus=8
    ...
    $ module load intel/13.0
    $ icc -openmp hello_omp.c -o hello.x
    $ export OMP_NUM_THREADS=8
    $ ./hello.x 

The above `hello.x` will create 8 threads.
