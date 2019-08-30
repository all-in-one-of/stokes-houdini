
# Stokes Variational Micro-Solver Plugin DOP for Houdini


## Building

Before building anything, set the EIGEN_INCLUDE_DIR environment variable to where you
installed the Eigen library in the Makefile.

SIM_Stokes.C and SIM_Stokes.h are the C++ files defining the Houdini plugin DOP.
In order to build them, source a houdini enviroment with

```
$ pushd /opt/hfs##.#.#/; source houdini_setup; popd
```

Then build with
```
$ make install
```

Note: instructions will differ for Windows


## Usage

To use the micro-solver,

1. Prepare your viscous fluid scene in Houdini.
2. Locate and dive into the **FLIP Solver** DOP node.
3. Change the type of the second **Gas Project Non Divergent Variational**
operator named `projectnondivergent` to the new **Stokes** node.
The parameters should stay mostly the same as in the original node.
  Note that **Min Viscosity** is the *dynamic* viscosity and NOT the kinematic viscosity
  from the **Gas Viscosity** node.
4. You may want to increase **Error Tolerance** by an
order of magnitude, and copy the **Samples Per Axis** parameter from the
**Gas Viscosity** node.
5. Remove the lingering spare parameters.
6. Finally, disconnect the remaining **Gas Project Non Divergent Variational** and **Gas
Viscosity** nodes named `projectnondivergent_viscosity` and `gasviscosity`
respectively.

Once disconnected, you can reap the benefits of the **Slip On
Collision** feature on the **FLIP Solver** when viscosity is enabled.  If viscosity
is disabled in the **FLIP Solver**, the resulting simulation will still exhibit
viscosity.  If you want to bypass the full Stokes solver without modifying the
**FLIP Solver** network further, simply select the **Pressure Only** **Scheme** on
the **Stokes** node to disable viscosity.

Note that currently, only the **Stokes** **Scheme** is fully optimized with OpenCL,
other Schemes will use a more expensive CPU based solver.


## Examples
To run the examples, `cd` into the `examples` directory and run
```
$ make <example name>
```
where `<example name>` is the name of the example you want to run. The list of available examples is shown in the `makefile` in the same directory. You can also run `make` without arguments to see this list.
The results will be produced in the `geo` or `sim` subdirectories or `render` for render targets.

The following examples are known to work:

- piling armadillos
