# Compiling applications in Mahti

## General instructions


- Whenever possible, use the [local disk](disk.md#login-nodes) on the login node for compiling software.
    - Compiling on the local disk is much faster and shifts load from the shared file system. 
    - The local disk is cleaned frequently, so please move your files elsewhere after compiling. 


## Building MPI applications

C/C++ and Fortran applications can be built with
[GNU](https://gcc.gnu.org), [AMD](https://developer.amd.com/amd-aocc/), 
or [Intel](https://software.intel.com/en-us/parallel-studio-xe/documentation/get-started)
compiler suites. The compiler suite is selected via the [Modules](modules.md)
system, i.e.
```
module load gcc
```
,
```
module load clang
```
or
```
module load intel
```

Different applications function better with different suites, so the selection
needs to be done on a case-by-case basis.

The MPI environment in Mahti is OpenMPI, and it needs to be loaded
after the compiler suite has been selected:
```
module load openmpi
```

When building MPI applications, all compiler suites can be used with
the `mpicc` (C), `mpicxx` (C++), or `mpif90` (Fortran) wrappers.

The compiler options for different suites are different. The
recommended basic optimization flags are listed in the table below. It
is recommended to start from 
the safe level and then move up to intermediate or even aggressive,
while making sure the results are  correct and the program's
performance has improved. 

FIXME: clang options, what are recommend Intel options?

| Optimisation level | GNU               | Intel                        | AMD          |
| :----------------- | :---------------- | :--------------------------- | :----------- |
| **Safe**           | -O2 -march=native | -O2 -xHost -fp-model precise | -O2          |
| **Intermediate**   | -O3 -march=native | -O2 -xHost                   | -O3          |
| **Aggressive**     | -O3 -march=native -ffast-math -funroll-loops | -O3 -xHost -fp-model fast=2 -no-prec-div -fimf-use-svml=true | -O3 |


A detailed list of options for the Intel and GNU compilers can be found on the _man_
pages (`man gcc/gfortran`, `man icc/ifort`)  when the corresponding programming
environment is loaded, or in the compiler manuals (see the links above).

List all available versions of the compiler suites:
```
module spider gcc
module spider clang
module spider intel
```

## Building OpenMP and hybrid applications

Additional compiler and linker flags are needed when building OpenMP or
MPI/OpenMP hybrid applications:

| Compiler suite | OpenMP flag |
| :------------- | :---------- |
| GNU and AMD    | -fopenmp    |
| Intel          | -qopenmp    |

## Building serial applications

For building serial applications, one needs to use compiler suite
specific compiler command:

| Compiler suite | C  | C++ | Fortran |
| :------------- | :- | :-- | :------ |
| GNU            | gcc | g++ | gfortran |
| AMD            | clang | clang++ | flang |
| Intel          | icc | icpc | ifort |