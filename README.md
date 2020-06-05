# DIABLO
DNS In A Box...Laptop Optimized!

## A brief description of DIABLO
|File | Description|
| --- | ---|
|diablo.f     |   Main program, define grid, read/write flow, statistics |
|input.dat     |  Set the desired flow parameters|
|fft.f       |    Wrapper for calls to FFTW|
|mpi.f 		| Subroutines for MPI parallelization|
|grid_def    |    Define the dimensions of the grid|
|header     |     Defines all variables and common blocks|

### Timestepping subroutines:
|File | Description|
| --- | ---|
|periodic.f   |   Periodic (spectral) in all three directions (done) |
|channel.f   |    Periodic (spectral) in x and z, FD in y (done)|
|duct.f       |   Periodic (spectral) in x, FD in z and y (not done) |
|cavity.f   |     FD in x, y, and z (not done)|

### User editable subroutines:
|File | Description|
| --- | ---|
|set_ics.f   |    Prescribe the initial conditions for velocity and scalars|
|save_stats.f |   Calculate statistics during the simulation|
|courant.f      | Set the dynamic timestep (optional) based on the CFL number|
|user_rhs.f   |   Add forcing terms to the right hand side for momentum and scalars|

## Runs
In order to run a new case do the following steps:
1.  Create a new run directory
2.  Copy the **grid*** and **input*** from an existing case directory and edit them as desired
3.  In the *diablo/pre_process* directory run **create_grid_*.m** in matlab or octave to create a new grid for each direction using finite differences (non periodic) and move the created ***grid.txt** files to the new case directory.
4.  Edit **set_ics.f** to set the desired initial conditions.
5.  Edit the script **“setup_run”** and set the *rundir* variable to match the new directory.
6.  Run **“setup_run”**.  This will create an executable called **“diablo”** in your run directory.
7.  Run the code from inside the run directory.  For example, if using MPI, you could do 
```mpirun -np diablo >output.dat &```
which will run a job in the background and write the output to the file output.dat
8.  Some post processing routines can be found in the *post_processing* directory along with descriptions on their use.

In finite difference (wall bounded) directions, the grid is defined as follows:
(Here the y-direction will be shown, but others should be treated the same)
The points outside the domain are ghost cells and are used to apply the desired boundary conditions.  In the case of one wall-bounded direction, the X and Z velocities (U1 and U3) are defined at the GYF points along with the pressure.  Vertical velocity (U2) is defined at GY points. The GYF points are by definition located halfway between neighboring GY points.

The table below attempts to graphically explain the computational domain in the Y direction.


||  | |
 | --- | :---: | --- |     
 |GYF(NY+1)    |     o    | | 
  |             |     *       |     GY(NY+1)|
 |GYF(NY)    |  -----Wall-----   | |
  |           |       *        |    GY(NY)|
 | GYF(NY-1)  |       o |
 | |                   *      |      GY(NY-1) |
 |GYF(NY-2)  |       o |
 |... | ... | ... |
  |GYF(j+1)      |    o | |
  | |                  *      |      GY(j+1)|
 |GYF(j)      |      o 
 | |                *     |       GY(j)|
 |GYF(j-1)     |     o |
  | |                  *    |        GY(j-1) |
| ... | ... | ...
| GYF(2)       |     o |
   | |                 *      |      GY(2)|
 | GYF(1)     | -----Wall-----
 | |                   *        |    GY(1) |
| GYF(0)   |         o | |

A full discription of the computational domain can be found below.

<object data="imgs/grid.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="imgs/grid.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="imgs/grid.pdf">Download PDF</a>.</p>
    </embed>
</object>

Enjoy! 

John

October, 2005  
