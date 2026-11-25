# Radiative shocks (Lowrie & Edwards 2008)

Steady grey non-equilibrium radiative shocks (Lowrie & Edwards 2008, Shock Waves 18, 129) run with
the production clean scheme: Mach 2 (subcritical) and Mach 5 (supercritical, with a Zel'dovich
spike), in cgs units and held in their own frame. Each run starts from the semi-analytic profile
(the `idort_radshock_m*.txt` files), keeps the far-field states on the x1 faces, and runs for
2e-10 s on 256 cells. The notebook plots the gas and radiation temperature and the density
against that profile.

The reference solves grey non-equilibrium diffusion, which uses the Eddington closure P = E/3.
The scheme uses its variable Eddington tensor from rays, so most of the ~1 % in T_rad is the
closure in the optically thin precursor. With `clean_closure = eddington` (the suite tests
`idort_radshock_m2` and `idort_radshock_m5`) the error falls to 2.5e-4.

| | |
|---|---|
| property shown | radiation hydrodynamics: precursor, shock and relaxation region of a coupled gas + radiation shock |
| suite criterion | every L1 of rho, v, T_gas, T_rad below 1-2 %; L1 of rho and T_rad within 5 % of the reference values; E > 0; no `CLEAN WARNING` |
| files | `radshock.ipynb`, `rad_radshock_m2.athinput`, `rad_radshock_m5.athinput`, `idort_radshock_m2.txt`, `idort_radshock_m5.txt` |
| time | about 30 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here. It runs the suite tests `radshock` and `radshock_m5` through
   `$ATHENAK/tests/vet_clean`, writes the runs to `runs/` and plots them:

   ```bash
   cd athenak-vet-gallery/radshock
   jupyter nbconvert --to notebook --execute --inplace radshock.ipynb
   ```

   You can also open it with `jupyter notebook radshock.ipynb` and run all cells. A test whose
   record `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the
   criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS radshock             18 s  L1_rho 0.00438 L1_v 0.00358 L1_Tg 0.00659 L1_Tr 0.0107 Emin 1.72e+11 warnings 0
PASS radshock_m5          15 s  L1_rho 0.00386 L1_v 0.00212 L1_Tg 0.0062 L1_Tr 0.00744 Emin 1.79e+11 warnings 0
```

The input decks and reference profiles here are copies for reading. The notebook runs the
decks in `$ATHENAK/tests/vet_clean/decks/` with the profiles in `$ATHENAK/tests/vet_clean/ref/`,
with the few keys the test changes on top (`run_tests.py` writes the deck it ran as
`in.athinput` in each run directory).
