---
title: Compilers available on Palmetto
keywords: [compiler, C, C++, Fortran, gcc, intel, PGI]
sidebar: documentation_sidebar
permalink: programmers_compilers.html
---

## Available Compilers

The Palmetto offers the following compiler suites
for C, C++ and Fortran applications:

1. GNU Compiler Collection (`gcc`)
2. Intel Compiler Suite
3. PGI Compiler Suite

~~~
$ module avail gcc
~~~

~~~
$ module avail intel
~~~

~~~
$ module avail pgi
~~~

## Compiling "Hello World" Program

### C

Simple `hello.c` file that reads

{% highlight cpp %}
#include <stdio.h>
 
int main(void){
    printf("Hello, world!\n");
    return 0;
}
{% endhighlight %}

can be compiled on Palmetto using the following commands

Compiler | Module        | Command
-------- | ------------- | --------------------
Intel    | `intel/13.0`  | `icc hello.c -o hello.x`
GCC      | `gcc/4.8.1`   | `gcc hello.c -o hello.x`
PGI      | `pgi/16.10`   | `pgcc hello.c -o hello.x`

### C++

The same example in C++ put in a file `hello.cpp`

{% highlight cpp %}
#include <iostream>
 
int main()
{
    std::cout << "Hello, world!" << std::endl;
    return 0;
}
{% endhighlight %}

can be compiled on Palmetto using the following commands

Compiler | Module        | Command
-------- | ------------- | --------------------
Intel    | `intel/13.0`  | `icpc hello.cpp -o hello.x`
GCC      | `gcc/4.8.1`   | `g++ hello.cpp -o hello.x`
PGI      | `pgi/16.10`   | `pgc++ hello.cpp -o hello.x`

### FORTRAN

To compile a simple FORTAN program `test.f90`

{% highlight fortran %}
program test
  print *,'hello from fortran'
end program test
{% endhighlight %}

use a command appropriate for a given compiler suite

Compiler | Module        | Command
-------- | ------------- | --------------------
Intel    | `intel/13.0`  | `ifort test.f90 -o test.x`
GCC      | `gcc/4.8.1`   | `gfortran test.f90 -o test.x`
PGI      | `pgi/16.10`   | `pgfortran test.f90 -o hello.x`

## GCC compiler options

Switch                      | Description
--------------------------- | ---------------------------------------------------------------------------------------------
`-c`                        | Compile the source file but do not link
`-x language`               | Set the specific language instead of letting the compiler decide based on the source file suffix. Useful for FORTRAN i.e. `language` can be replaced with  `f77`, `f77-cpp-input`, `f95` or `f95-cpp-input`
`-o file`                   | Change the name of the binary file from `a.out` to `file`
`-v`                        | Print version of the compiler with options used for its configuration
`-fopenmp`                  | Enables OpenMP directives to create a multithreaded code
`-fno-gnu-keywords`         | Turns off GNU specific language extensions
`-w`                        | Suspend add warnings 
`-Wall`                     | Enables all warnings 
`-g`                        | Enables debugging information
`-p` or `-pg`               | Turns on `gprof` profiling information
`-ftree-vectorizer-verbose` | Enables verbose mode for GCC vectorization
`-O`, `-O1`, `-O2` or `-O3` | Different levels of code optimization; -O3 is the most aggressive
`-Ofast`                    | `-Ofast` enables all `-O3` optimizations.  It also enables optimizations that are not valid for all standard-compliant programs
`-Og`                       | Turns on debugging safe optimization mode
`-floop-block`              | Perform loop blocking transformations on loops
`-floop-interchange`        | Perform loop interchange transformations on loops
`-ftree-vectorize`          | Perform loop vectorization on trees. This flag is enabled by default at -O3
`-funroll-loops`            | Unroll loops whose number of iterations can be determined at compile time or upon entry to the loop
`-fprefetch-loop-arrays`    | Generate instructions to prefetch memory to improve the performance of loops that access large arrays
`-ffast-math`               | Use faster but less accurate mathematical functions
`-D`                        | Define a macro
`-I<dir>`                   | Add `<dir>` to compilers path for included files 
`-l<lib>`                   | Pass a library `<lib>` to the linker
`-L<dir>`                   | Add directory `<dir>` to the linker library path
`-march=native`             | This selects the CPU to generate code for at compilation time by determining the processor type of the compiling machine
`-ffree-form`               | FORTRAN : Specify the layout used by the source file
`-cpp`                      | FORTRAN : Enable preprocessor for FORTRAN files
`-fno-underscoring`         | FORTRAN : Do not transform names of entities specified in the Fortran source file by appending underscores to them
`-fexternal-blas`           | FORTRAN : This option will make gfortran generate calls to BLAS functions for some matrix operations like "MATMUL"
`-floop-parallelize-all`    | Identify loops that can be parallelized
`-ftree-parallelize-loops=n`| Parallelize loops, i.e., split their iteration space to run in n threads. This option implies `-pthread`

More information 

* GCC autoparallelization <http://gcc.gnu.org/wiki/Graphite/Parallelization>

## Intel compiler options

Switch                      | Description
--------------------------- | --------------------------------------------------------------------------------------------
`-parallel`                 | enable the auto-parallelizer to generate multi-threaded code for loops that can be safely executed in parallel 
`-par-reportX`              | (X=1,2,3) control the auto-parallelizer diagnostic level
`-openmp`                   | enable the compiler to generate multi-threaded code based on the OpenMP* directives
`-openmp-reportX`           | (X=1,2,3) control the OpenMP parallelizer diagnostic level
`-fast`                     | enable `-xHOST -O3 -ipo -no-prec-div -static` options set by -fast cannot be overridden with the exception of `-xHOST`, list options separately to change behavior
`-O0`                       | disable optimizations
`-O1`                       | optimize for maximum speed, but disable some optimizations which increase code size for a small speed benefit
`-O2`                       | optimize for maximum speed (DEFAULT)
`-O3`                       | optimize for maximum speed and enable more aggressive optimizations that may not improve performance on some programs
`-threads`                  | specify that multi-threaded libraries should be linked against `-nothreads` disables multi-threaded libraries
`-vec`                      | enables (DEFAULT) vectorization (`-novec` disables vectorization)
`-mkl`                      | link to the Math Kernel Library (MKL) and bring in the associated headers, `-mkl=` one of `parallel` (default), `sequential` or `cluster`
`-opt-matmul`               | replace matrix multiplication with calls to intrinsics and threading libraries for improved performance (DEFAULT at `-O3 -parallel`)
`-xHost`                    | generate instructions for the highest instruction set and processor available on the compilation host machine
`-integer-size XX`          | specifies the default size of integer and logical variables size:  16, 32, 64
`-real-size XX`             | specify the size of REAL and COMPLEX declarations, constants, functions, and intrinsics size: 32, 64, 128
`-w`                        | disable all warnings, equivalent to `-warn none` or `-nowarn`
`-warn all`                 | enables all warnings
`-L<dir>`                   | instruct linker to search `<dir>` for libraries
`-l<string>`                | instruct the linker to link in the `-l<string>` library
`-Wl,<o1>[,<o2>,...]`       | pass options `o1`, `o2`, etc. to the linker for processing
`-V`                        | display compiler version information
`-multiple-processes[=<n>]` | create multiple processes that can be used to compile large numbers of source files at the same time
`-c`                        | compile to object (.o) only, do not link
`-S`                        | compile to assembly (.s) only, do not link
`-o <file>`                 | name output executable file
`-g`                        | produce symbolic debug information in object file (implies `-O0` when another optimization option is not explicitly set)
`-p`                        | compile and link for function profiling with UNIX gprof tool, `-pg` is also valid
`-D<name>[=<text>]`         | define macro
`-I<dir>`                   | add directory to include file search path
`-fpp`                      | run Fortran preprocessor on source files prior to compilation
