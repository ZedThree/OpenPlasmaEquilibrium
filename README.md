# Open Plasma Equilibrium

The Open Plasma Equilibrium (OPEQ) file format is a specification for
plasma equilibria using [HDF5][hdf5]. Currently a work in progress,
absolutely everything about this is subject to change, including the
name.

OPEQ is licensed under Creative Commons Attribution Share Alike 4.0
International. See [LICENSE.md](LICENSE.md).

## TODO

- Provide schema and validator
- Provide units for all quantities

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

### Metadata (in root `/`)

| Variable             | Required | Rank | Type     | Units | Description                                                    |
|:---------------------|:---------|------|:---------|:------|:---------------------------------------------------------------|
| `/version`           | yes      | n/a  | `string` | n/a   | Version number of the OPEQ specification this file conforms to |
| `/creation_software` | yes      | n/a  | `string` | n/a   | Name of the software used to create this file                  |
| `/creation_version`  | yes      | n/a  | `string` | n/a   | Version of the software used to create this file               |
| `/symmetry`          | no       | n/a  | `string` | n/a   | Type of symmetry. Only "tokamak" is currently supported        |

### Equilibrium quantities (in group `/equilibrium`)

| Variable                   | Required       | Rank | Type     | Units  | Description                                                                                           |
|:---------------------------|:---------------|------|:---------|:-------|:------------------------------------------------------------------------------------------------------|
| `Rmin`                     | no             | 0    | `float`  | metres | Minimum major radius                                                                                  |
| `Rmax`                     | no             | 0    | `float`  | metres | Maximum major radius                                                                                  |
| `R_1D`                     | yes            | 1    | `float`  | metres | Major radius                                                                                          |
| `R`                        | no<sup>1</sup> | 2    | `float`  | metres | Major radius                                                                                          |
| `Zmin`                     | no             | 0    | `float`  | metres | Minimum vertical coordinate                                                                           |
| `Zmax`                     | no             | 0    | `float`  | metres | Maximum vertical coordinate                                                                           |
| `Z_1D`                     | yes            | 1    | `float`  | metres | Vertical coordinate                                                                                   |
| `Z`                        | no<sup>1</sup> | 2    | `float`  | metres | Vertical coordinate                                                                                   |
| `boundary_function`        | no             | n/a  | `string` | n/a    | Name of boundary function to apply. One of `["fixedBoundary", "freeBoundary", "freeBoundaryHagenow"]` |
| `current`                  | no             | 0    | `float`  | Amps   | Plasma current                                                                                        |
| `psi`                      | yes            | 2    | `float`  | FIXME  | Total poloidal flux, including contribution from plasma and external coils as a function of `(R, Z)`  |
| `plasma_psi`               | yes            | 2    | `float`  | FIXME  | Poloidal flux, just contribution from plasma, as a function of `(R, Z)`                               |
| `pressure`                 | no             | 2    | `float`  | FIXME  | Pressure as a function of `(R, Z)`                                                                    |
| `toroidal_current_density` | no             | 2    | `float`  | FIXME  | Toroidal current density as a function of `(R, Z)`                                                    |

<sup>1</sup>.: If one of these variables is present, they must both be present

### Machine information (in group `/equilibrium/<symmetry>`)

| Variable                  | Required       | Rank | Type    | Units    | Description                                                      |
|:--------------------------|:---------------|------|:--------|:---------|:-----------------------------------------------------------------|
| `wall_R`                  | no<sup>2</sup> | 1    | `float` | metres   | Major radius locations of points defining the machine wall       |
| `wall_Z`                  | no<sup>2</sup> | 1    | `float` | metres   | Vertical locations of points defining the machine wall           |
| `coils/<name>/R`          | no<sup>3</sup> | 0    | `float` | metres   | Major radius location of the coil                                |
| `coils/<name>/Z`          | no<sup>3</sup> | 0    | `float` | metres   | Vertical location of the coil                                    |
| `coils/<name>/area`       | no<sup>3</sup> | 0    | `float` | metres^2 | Cross-sectional area of the coil                                 |
| `coils/<name>/current`    | no<sup>3</sup> | 0    | `float` | Amps     | Current in each turn of the coil                                 |
| `coils/<name>/turns`      | no<sup>3</sup> | 0    | `int`   | n/a      | Number of turns in coil                                          |
| `coils/<name>/control`    | no<sup>3</sup> | 0    | `bool`  | n/a      | Control system is enabled                                        |
| `coils/<name>/multiplier` | no<sup>4</sup> | 0    | `float` | n/a      | If this coil is part of a circuit, the multiplier of the current |
|                           |                |      |         |          |                                                                  |

<sup>2</sup>: If one of these variables is present, they must both be
present  
<sup>3</sup>: If one of these variables is present, they must all be
present  
<sup>4</sup>: This must be present if and only if this coil is part of a circuit

## Attributes

All variables may have the following attributes:

| Attribute     | Type     | Description                |
|:--------------|----------|:---------------------------|
| `title`       | `string` | Short name of the variable |
| `description` | `string` | Long name of the variable  |
| `units`       | `string` | SI unit name               |

Variables which have units specified must have the `units` attribute.


[hdf5]: https://www.hdfgroup.org/
