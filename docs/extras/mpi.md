## Parallel computing with FUSE

When running FUSE over large (e.g. greater than 10'000 grid cells) domains, we recommend distributing the simulations over multiple cores using FUSE MPI (Message Passing Interface) feature. To enable MPI, compile FUSE as follows:

`make FC=your_compiler MODE=distributed`

With the current Makefile, replacing `your_compiler` by `ifort` or `gfortran` will result in `mpif90` and `mpiifort` being used, respectively (we  have tested both successfully). The resulting executable will be `fuse_mpi.exe`. You will need to specify how many processors (`np`) FUSE can use, the command is system dependent, here is an example for 16 processors - you are likely to need additional arguments for it to run on your system:

`mpirun -np 16 ./fuse_mpi.exe fm_900_conus_ca_mx_5d.txt maurer_ca_mx_1980_1989 run_def > log_5d.txt`

The simulations will be stored in `np` individual files (horizontal transects of the domain), which can be stitch together using `xarray` (note that this step is memory intensive):

`python -c 'import xarray as xr; xr.open_mfdataset("*runs*",combine="by_coords").to_netcdf("combined_run.nc")'`
