# Open Plasma Equilibrium

The Open Plasma Equilibrium (OPEQ) file format is a specification for
plasma equilibria using [HDF5][hdf5]. Currently a work in progress,
absolutely everything about this is subject to change, including the
name.

## OPEQ Specification 0.1.0

- OPEQ files are HDF5 files
    - Specify HDF5 version? At least 1.10? Backwards compatibility
      issues
- Files must have some metadata: what version of OPEQ spec, what
  software was used to create the file (including that software
  version)
- Currently targeting only tokamaks, but for future-proofing, this can
  be specified via `symmetry` variable
- `<name>` is placeholder for a value
- `/equilibrium/<symmetry>` must match the value of `/symmetry`, that
  is `/equilibrium/tokamak` is currently the only supported group name
- Circuits consisting of multiple coils are represented by nested
  groups:
  - `/equilibrium/tokamak/coils/P2/P2U` and
    `/equilibrium/tokamak/coils/P2/P2L` are two coils that make up the
    `P2` circuit


| Variable                                          | Required       | Rank | Type     | Units    | Description                                                                                                                          |
|:--------------------------------------------------|:---------------|------|:---------|:---------|:-------------------------------------------------------------------------------------------------------------------------------------|
| `/version`                                        | yes            | n/a  | `string` | n/a      | Version number of the OPEQ specification this file conforms to                                                                       |
| `/creation_software`                              | yes            | n/a  | `string` | n/a      | Name of the software used to create this file                                                                                        |
| `/creation_version`                               | yes            | n/a  | `string` | n/a      | Version of the software used to create this file                                                                                     |
| `/symmetry`                                       | no             | n/a  | `string` | n/a      | Type of symmetry. Only "tokamak" is currently supported                                                                              |
| `/equilibrium/Rmin`                               | no             | 0    | `float`  | metres   | Minimum major radius                                                                                                                 |
| `/equilibrium/Rmax`                               | no             | 0    | `float`  | metres   | Maximum major radius                                                                                                                 |
| `/equilibrium/R_1D`                               | yes            | 1    | `float`  | metres   | Major radius                                                                                                                         |
| `/equilibrium/R`                                  | no             | 2    | `float`  | metres   | Major radius                                                                                                                         |
| `/equilibrium/Zmin`                               | no             | 0    | `float`  | metres   | Minimum vertical coordinate                                                                                                          |
| `/equilibrium/Zmax`                               | no             | 0    | `float`  | metres   | Maximum vertical coordinate                                                                                                          |
| `/equilibrium/Z_1D`                               | yes            | 1    | `float`  | metres   | Vertical coordinate                                                                                                                  |
| `/equilibrium/Z`                                  | no             | 2    | `float`  | metres   | Vertical coordinate                                                                                                                  |
| `/equilibrium/boundary_function`                  | no             | 0    | `string` | n/a      | Name of boundary function to apply. One of `["fixedBoundary", "freeBoundary", "freeBoundaryHagenow"]`                                |
| `/equilibrium/current`                            | no             | 0    | `float`  | Amps     | Plasma current                                                                                                                       |
| `/equilibrium/psi`                                | yes            | 2    | `float`  | FIXME    | Total poloidal flux, including contribution from plasma and external coils as a function of `(R, Z)`                                 |
| `/equilibrium/plasma_psi`                         | yes            | 2    | `float`  | FIXME    | Poloidal flux, just contribution from plasma, as a function of `(R, Z)`                                                              |
| `/equilibrium/pressure`                           | no             | 2    | `float`  | FIXME    | Pressure as a function of `(R, Z)`                                                                                                   |
| `/equilibrium/toroidal_current_density`           | no             | 2    | `float`  | FIXME    | Toroidal current density as a function of `(R, Z)`                                                                                   |
| `/equilibrium/<symmetry>/wall_R`                  | no             | 1    | `float`  | metres   | Major radius locations of points defining the machine wall                                                                           |
| `/equilibrium/<symmetry>/wall_Z`                  | no             | 1    | `float`  | metres   | Vertical locations of points defining the machine wall                                                                               |
| `/equilibrium/<symmetry>/coils/<name>/R`          | no<sup>1</sup> | 0    | `float`  | metres   | Major radius location of the coil                                                                                                    |
| `/equilibrium/<symmetry>/coils/<name>/Z`          | no<sup>1</sup> | 0    | `float`  | metres   | Vertical location of the coil                                                                                                        |
| `/equilibrium/<symmetry>/coils/<name>/area`       | no<sup>1</sup> | 0    | `float`  | metres^2 | Cross-sectional area of the coil                                                                                                     |
| `/equilibrium/<symmetry>/coils/<name>/current`    | no<sup>1</sup> | 0    | `float`  | Amps     | Current in each turn of the coil                                                                                                     |
| `/equilibrium/<symmetry>/coils/<name>/turns`      | no<sup>1</sup> | 0    | `int`    | n/a      | Number of turns in coil                                                                                                              |
| `/equilibrium/<symmetry>/coils/<name>/control`    | no<sup>1</sup> | 0    | `bool`   | n/a      | Control system is enabled                                                                                                            |
| `/equilibrium/<symmetry>/coils/<name>/multiplier` | no<sup>1</sup> | 0    | `float`  | n/a      | If this coil is part of a circuit, the multiplier of the current. This must be present if and only if this coil is part of a circuit |
|                                                   |                |      |          |          |                                                                                                                                      |

<sup>1</sup>: If one of these variables is present, they must all be present

All variables may have the following attributes:

| Attribute     | Type     | Description                |
|:--------------|----------|:---------------------------|
| `title`       | `string` | Short name of the variable |
| `description` | `string` | Long name of the variable  |
| `units`       | `string` | SI unit name               |



[hdf5]: https://www.hdfgroup.org/
