# Diabat

Diabat is a computational toolkit for diabatization and related electronic-structure post-processing of multichromophoric systems.

The public Diabat v2.0 release is distributed as a Linux binary package. This release focuses on three major modules:

* `diabat`: diabatization methods for electronic states;
* `postorb`: post-processing and analysis of electronic orbitals;
* `postwfn`: post-processing and analysis of electronic-state wave functions.

Other research components under development are not included as public user modules in this binary release.

## Main capabilities in v2.0

### Diabatization

The `diabat` module provides several diabatization methods and utilities, including:

* `diabat-fphd` Fragment Particle-Hole Densities method for multichromophoric systems.
* `diabat-gmh` (Multistate) generalized Mulliken-Hush method for charge transfer in dimer systems.
* `diabat-fed` Fragment Excitation Difference method for excitation energy transfer in dimer systems.
* `diabat-fcd` (Multistate) fragment Charge Difference method for charge transfer in dimer systems.
* `diabat-fedfcd` Multistate FED-FCD method for dimer systems involving both excitation-energy-transfer and charge-transfer characters.
* `diabat-utils-phasefix` Reference-based phase alignment utility for electronic-state wave functions.

Fragment-based diabatization methods support both Mulliken and Löwdin population partitioning, where the latter one is the default option.

### Orbital post-processing

The `postorb` module provides tools for orbital-related analysis and visualization, including orbital information, orbital integrals, and spatial grid data for orbital wave functions and electron densities.

### Electronic-state post-processing

The `postwfn` module provides tools for analyzing electronic-state wave functions, including density-matrix, transition-density-matrix, and difference-density-matrix based analyses. It can be used to generate natural orbitals, natural transition orbitals, natural difference orbitals, transition properties, and spatial grid data for densities, transition densities, hole/particle densities, and attachment/detachment densities.

Diabat v2.0 includes several corrections and robustness improvements in electronic-state and transition-density analyses.

## Download

Please download the binary package from the Releases page of this repository.

For Diabat v2.0, the Linux x86-64 package is:

```text
diabat-v2.0-linux-x86_64.tar.gz
```

A SHA-256 checksum file is also provided with the release.

## Installation

After downloading the binary package:

```bash
tar -xzf diabat-v2.0-linux-x86_64.tar.gz
cd diabat-v2.0-linux-x86_64
bash install.sh
```

The installation script will:

1. update the Diabat path used by the example input files;
2. make the main executable runnable;
3. check whether required runtime libraries can be found;
4. print the command that should be added to your shell configuration.

The installer does not modify your `.bashrc` automatically. After installation, add the printed `bin/` path manually, for example:

```bash
export PATH="/path/to/diabat/bin:$PATH"
```

Then reload your shell configuration:

```bash
source ~/.bashrc
```

or open a new terminal.

## Runtime environment

The v2.0 binary was built for Linux x86_64 and dynamically links to Intel runtime libraries, including Intel MPI, Intel MKL, Intel OpenMP runtime, and Intel Fortran runtime libraries.

Before running Diabat, make sure the required Intel runtime environment is available on your system. On an HPC cluster, this may require commands such as:

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

Run a Diabat job with an input file:

```bash
diabat input.inp
```

For an MPI run (recommended):

```bash
mpirun -np 8 diabat input.inp
```

For a hybrid MPI/OpenMP run:

```bash
export OMP_NUM_THREADS=4
mpirun -np 8 diabat input.inp
```

## Input files and examples

Diabat uses a block-structured script input format. A script file is organized as a tree of named blocks.

A block starts with one or more `$` markers followed by the block name, and ends with a line containing the same number of `$` markers. The number of `$` markers indicates the block level:

```text
$block     # first-level block
  $$subblock    # second-level block
  $$
$
```

A block may contain keywords and subblocks. Keywords are written in the form:

```text
keyword = value
```

Everything after `#` on the same line is treated as a comment.

For example, a simple diabatization input may contain:

```text
$diabat-fphd
  # Electronic-state data
  $$statepack
    mofile = 'path/to/orbital/file'
    statefile = 'path/to/state/file'
    state_index = 1..8
  $$

  # Fragment definition
  $$frag
    num_frag = 2
    frag 1 = 1..30
    frag 2 = 31..60
  $$
$
```

Examples are provided in the `examples/` directory of the binary package. The corresponding raw data files are stored in related data directories.

Users are encouraged to start from the provided examples and modify the input files for their own molecular systems.

The examples included in v2.0 cover additional molecular systems, electronic-structure methods, spin cases, and fragment-partitioning options compared with v1.9.8.

## Package contents

A typical Diabat v2.0 binary package contains:

```text
bin/
  diabat

examples/
  ...

data/
  ...

script/
  ...

licenses/
  ...

install.sh
BUILDINFO.txt
VERSION.txt
README.md
```

`BUILDINFO.txt` records the compiler, MPI library, MKL information, build options, and key runtime dependencies of the binary package.

## Changes in v2.0

Compared with v1.9.8, Diabat v2.0 includes several improvements.

### Diabatization

* Added Mulliken fragment partitioning as an alternative to Löwdin partitioning for applicable fragment-based diabatization methods.
* Extended the FPHD implementation to support general fragment partitioning schemes.
* Added unified control of partial Hamiltonian diagonalization through the `partial_diag` option for FED, FCD, FED-FCD, GMH, and FPHD calculations.
* Added output of property matrices used in several diabatization procedures, including fragment charge, fragment excitation, and projected dipole matrices where applicable.
* Improved convergence handling and reporting in FPHD calculations.
* Improved phase alignment of diabatic states when input electronic states are not supplied in energetic order.

### Correctness and robustness

* Corrected the construction of TDDFT transition-density matrices.
* Corrected fragment excitation populations in the two-state FED procedure.
* Corrected spin handling in natural difference orbital analysis.
* Improved handling of reordered or non-sequential electronic-state selections.
* Improved Molden-format detection so that recognition does not rely solely on the filename extension.

## License

Diabat v2.0 is distributed under the PolyForm Noncommercial License 1.0.0.

Commercial use is not permitted without separate permission from the author. Please see the `licenses/LICENSE.txt` file for the full license text.

## Citation

If you use Diabat in published work, please cite:

> Y.-C. Wang, Y. Qin, Y. Zhao, W. Z. Liang.
> Diabat 2.0: A Comprehensive Diabatization Toolkit for Multichromophoric Systems.
> Journal of Chemical Theory and Computation, [year], [volume], [pages].
> DOI: [DOI]

If you use the Fragment Particle-Hole Densities method, please also cite:

> Y.-C. Wang et al., Journal of Physical Chemistry Letters 2021, 12, 1032–1039.

> Y.-C. Wang et al., Journal of Chemical Theory and Computation 2023, 19, 3900–3914.

Additional method-specific references may be printed by Diabat when running individual modules.

## Contact

For documentation, updates, bug reports, and feature requests, please visit:

```text
https://github.com/laughtale-lab/diabat/
```

## Release note

Diabat v2.0 is a public binary release focused on diabatization and electronic-structure post-processing. It provides updated functionality, correctness fixes, and robustness improvements for the `diabat`, `postorb`, and `postwfn` modules.
