# AthenaK VET radiation gallery

Worked problems for the clean VET radiation scheme of
[AthenaK](https://github.com/jing-ze-ma/athenak) (branch `vet-clean-release`): one folder per
problem, each with a README with the steps, the input deck(s) and a Jupyter notebook, executed,
with its plots. The documentation is at
[athenak-vet-docs](https://jing-ze-ma.github.io/athenak-vet-docs/). The layout follows the
[IAS AthenaK gallery](https://github.com/IAS-Astrophysics/athenak-gallery).

## Tutorials

- **[3-D star, spherical polar](star3d-sp)**: a blue-supergiant envelope on a 60 deg wedge with
  the production keys; a few cycles on one CPU node, then 15 min on 4 GPUs; luminosity, slices
  and profiles.
- **[3-D star, cubed sphere](star3d-cs)**: a 20.5 Msun red supergiant on the full sphere; the same
  steps.
- **[Conservation](conservation)**: energy, mass and L_z on a closed rotating shell (spherical
  polar and cubed sphere).
- **[Restart](restart)**: straight vs restarted runs are identical.
- **[Radiation-acoustic waves](radwave)**: phase speed and damping from the free-streaming to the
  trapped limit against the analytic root.
- **[Narrow wedges](wedge_sym)**: phi mirror symmetry on 3.7, 7.3 and 20 deg wedges.
- **[Well-balancing](well_balance)**: an envelope in hydrostatic and radiative equilibrium stays
  at rest.
- **[Order of accuracy](order)**: the measured orders in time and space.
- **[Thermal equilibrium](thermal_eq)**: gas-radiation exchange against the exact ODE.
- **[Non-LTE atmosphere](nlte)**: the ray closure against the exact transport solution.
- **[Radiative shocks](radshock)**: Lowrie & Edwards Mach 2 and 5 shocks against the
  semi-analytic profiles.
- **[Linear momentum](momentum_box)**: the momentum drift in a periodic box (a report).
- **[Crossing beams and shadow](beams_shadow)**: beams crossing in vacuum and the shadow of an
  opaque cloud against the exact beam fields (Jiang 2021; Ma et al. 2025).
- **[Emitting cylinder](cylinder)**: E and F inside and outside an optically thick emitting
  cylinder against the exact transport solution.
- **[Floors](floor_pay)**: floors with a general EOS, paid from the radiation.

## Setup (once)

The test notebooks run their test through the suite of the code repository
(`tests/vet_clean`), so they need a checkout and its CPU build. Requirements: a C++17
compiler, MPI, CMake 3.20 or newer, python3 with numpy, matplotlib and jupyter.

Get the code and build the CPU test binary (block 3a of the
[Build](https://jing-ze-ma.github.io/athenak-vet-docs/build.html) page, the `build_tests`
lines; about 2 min with 16 make jobs):

```bash
git clone https://github.com/jing-ze-ma/athenak.git athenak
cd athenak
git checkout vet-clean-release
git submodule update --init kokkos
OPTS="-D CMAKE_BUILD_TYPE=Release -D CMAKE_CXX_COMPILER=mpicxx
  -D Athena_ENABLE_MPI=ON -D Athena_ENABLE_OPENMP=ON
  -D CMAKE_CXX_FLAGS=-ffp-contract=off -D Athena_VET_VGD_NMAX=4096"
cmake -S . -B build_tests $OPTS
make -C build_tests -j 16 athena
cd ..
```

Get this gallery next to it and name the two in the environment:

```bash
git clone https://github.com/jing-ze-ma/athenak-vet-gallery.git
export ATHENAK=$PWD/athenak
export ATHENA_EXE=$ATHENAK/build_tests/src/athena
```

Then follow the README of a folder. The notebooks read `ATHENAK` (default `../../athenak`,
a checkout next to the gallery) and `ATHENA_EXE` (default `$ATHENAK/build_tests/src/athena`);
`M1_NB_THREADS` sets the OpenMP threads per run (default 4). The two star tests need GPU
builds instead; their READMEs say how.
