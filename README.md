# Diabat

**Diabat** is a computational toolkit for diabatization and related electronic-structure post-processing of multichromophoric systems.

The public **Diabat v1.9.8** release is distributed as a Linux binary package. This release focuses on three major user-facing parts:

* `diabat`: diabatization methods for electronic states;
* `postorb`: post-processing and analysis of electronic orbitals;
* `postwfn`: post-processing and analysis of electronic-state wave functions.

Other research components under development are not included as public user modules in this binary release.

## Main capabilities in v1.9.8

### Diabatization

The `diabat` module provides several diabatization methods and utilities, including:

* `diabat-fphd`
  Fragment Particle-Hole Densities method for multichromophoric systems.

* `diabat-gmh`
  Generalized Mulliken-Hush method for charge-transfer problems.

* `diabat-fed`
  Fragment Excitation Difference method for excitation energy transfer in dimer systems.

* `diabat-fcd`
  Fragment Charge Difference method for charge transfer in dimer systems.

* `diabat-fedfcd`
  Multistate FED-FCD method for dimer systems involving both excitation-energy-transfer and charge-transfer characters.

* `diabat-utils-phasefix`
  Reference-based phase alignment utility for electronic-state wave functions.

### Orbital post-processing

The `postorb` module provides tools for orbital-related analysis and visualization, including orbital information, orbital integrals, and spatial grid data for orbital wave functions and electron densities.

### Electronic-state post-processing

The `postwfn` module provides tools for analyzing electronic-state wave functions, including density-matrix, transition-density-matrix, and difference-density-matrix based analyses. It can be used to generate natural orbitals, natural transition orbitals, natural difference orbitals, transition properties, and spatial grid data for densities, transition densities, hole/particle densities, and attachment/detachment densities.

## Download

Please download the binary package from the **Releases** page of this repository.

For **Diabat v1.9.8**, the package is expected to have a name similar to:

```bash
diabat-v1.9.8-linux-x86_64.tar.gz
```

## Installation

After downloading the binary package:

```bash
tar -xzf diabat-v1.9.8-linux-x86_64.tar.gz
cd diabat-v1.9.8-linux-x86_64
bash install.sh
```

The installation script will:

1. update the **Diabat** path used by the example input files;
2. make the main executable runnable;
3. check whether required runtime libraries can be found;
4. print the command that should be added to your shell configuration.

The installer does **not** modify your `.bashrc` automatically. After installation, add the printed `bin/` path manually, for example:

```bash
export PATH=/path/to/diabat/bin:$PATH
```

Then reload your shell configuration:

```bash
source ~/.bashrc
```

or open a new terminal.

## Runtime environment

The v1.9.8 binary was built for Linux x86_64 and dynamically links to Intel runtime libraries, including Intel MPI, Intel MKL, Intel OpenMP runtime, and Intel Fortran runtime libraries.

Before running **Diabat**, make sure the required Intel runtime environment is available on your system. On an HPC cluster, this may require commands such as:

```bash
module load intel
module load intel-mpi
module load mkl
```

The exact module names depend on your local computing environment.

## Basic usage

After installation, check the available help and usage information:

```bash
diabat --help
diabat --usage
```

Run a **Diabat** job with an input file:

```bash
diabat input.inp
```

For an MPI run:

```bash
mpirun -np 8 diabat input.inp
```

For a hybrid MPI/OpenMP run:

```bash
export OMP_NUM_THREADS=4
mpirun -np 8 diabat input.inp
```

For pure MPI calculations, it is usually recommended to set:

```bash
export OMP_NUM_THREADS=1
```

## Input files and examples

**Diabat** uses a block-structured script input format. A typical job block starts with a `$` marker and ends with `$`.

For example, a diabatization input may contain a block such as:

```text
$diabat-fphd
  ...
$
```

Examples are provided in the `examples/` directory of the binary package. The corresponding raw data files are stored in related data directories.

Users are encouraged to start from the provided examples and modify the input files for their own molecular systems.

## Package contents

A typical **Diabat** v1.9.8 binary package contains:

```text
bin/
  diabat

examples/
  ...

data/
  ...

licenses/
  ...

install.sh
BUILDINFO.txt
VERSION.txt
README.md
```

`BUILDINFO.txt` records the compiler, MPI library, MKL information, build options, and key runtime dependencies of the binary package.

## License

**Diabat v1.9.8** is distributed under the **PolyForm Noncommercial License 1.0.0**.

Commercial use is not permitted without separate permission from the author. Please see the `licenses/LICENSE.txt` file for the full license text.

## Citation

If you use **Diabat** in published work, please cite:

> Y.-C. Wang, Y. Qin, Y. Zhao, W. Z. Liang.<br>
> *Diabat 2.0: A Comprehensive Diabatization Toolkit for Multichromophoric Systems.*<br>
> *Journal of Chemical Theory and Computation*, [year], [volume], [pages].<br>
> DOI: [DOI]

If you use the Fragment Particle-Hole Densities method, please also cite:

> Y.-C. Wang et al., *Journal of Physical Chemistry Letters* **2021**, *12*, 1032–1039.<br>
> Y.-C. Wang et al., *Journal of Chemical Theory and Computation* **2023**, *19*, 3900–3914.

Additional method-specific references may be printed by **Diabat** when running individual modules.

## Contact

For documentation, updates, bug reports, and feature requests, please visit:

```text
https://github.com/laughtale-lab/diabat/
```

## Release note

**Diabat** v1.9.8 is a public binary release focused on diabatization and electronic-structure post-processing. It is intended to provide stable user access to the `diabat`, `postorb`, and `postwfn` modules.
