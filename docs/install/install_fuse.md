## Notes on the computing environment

This page will guide you through the installation of FUSE. Before you get started, please note that:

1. below we assume that you will be compiling and running FUSE in a Linux/UNIX environment - for OS X/macOS, see this [page](https://summa.readthedocs.io/en/latest/installation/SUMMA_on_OS_X/) of the SUMMA manual,
2. you will need a Fortran compiler: FUSE was developed and tested using `ifort`, which we recommend if you have no previous experience with Fortran compilers - note that on HPCs, you might have to load specific modules to use the compiler (in which case, try `module avail` and then `module add [your/module/version/compiler]`),
3. you will need access to the NetCDF and HDF libraries: use the libraries compiled with the compiler you selected above, the path to these libraries are machine dependent (so paths for another machine probably will not work on your machine). To find these paths, ask  administrators or users of your machine, or, if you have to load modules containing the libraries, once loaded, type `module show [your/module/version/compiler]`.

## 1. Fork the FUSE repository and adapt the Makefile
1. Fork the [FUSE repository](https://github.com/naddor/fuse) to a directory `$(MASTER)` on your machine (see the [SUMMA manual](http://summa.readthedocs.io/en/latest/development/SUMMA_and_git/) for a step-by-step procedure).
2. Edit the `Makefile` in `$(MASTER)/build/` by defining:
    * the name of the master directory (line 10),
    * the fortran compiler (lines 31-32, optional, we recommend that you define it when compiling the code, see 2. above),
    * the path to the NetCDF and HDF libraries (`NCDF_PATH` and `HDF_PATH`, lines 38-46, see 3. above, provide the paths associated with the compiler you selected).

## 2. Compile FUSE
In spring 2020, we spruced up the FUSE Makefile. Until then, it used to require the separate compilation of the shuffled complex evolution (SCE, used for automated parameter estimation), as SCE code is in Fortran77. Now, SCE compilation is taken care of by the Makefile. To compile FUSE:

1. Change directory to `$(MASTER)/build/` and compile FUSE by typing `make FC=ifort` (or `make FC=gfortran` if you prefer to use `gfortran`).

2. Try running FUSE by typing `./fuse.exe`. If the output is `1st command-line argument is missing (fileManager)`, you have probably compiled FUSE correctly. But there might still be issues related to the libraries. To find out, download the [test data](../test_data/) and run the [test cases](../test_cases/).

<a id="infile_file_formats"></a>
