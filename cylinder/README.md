# Homogeneous emitting cylinder

A uniform emitting cylinder (r < 1, rho kappa = 10, a T^4 = 1) in vacuum on a Cartesian mesh,
the 2-D analogue of the homogeneous sphere (Ma et al. 2025, IDORT, sec. 4.1.2; Jiang 2021,
ApJS 253, 49, sec. 4.6). The notebook plots E and the flux along y = 0 against the exact
transport solution (`idort_cylinder_exact.npz`).

| | |
|---|---|
| property shown | closure accuracy (VET from rays) around an emitting body |
| suite criterion | L1 of E inside and outside and of F outside within 5 % of the reference values |
| files | `cylinder.ipynb`, `idort_cylinder.athinput`, `idort_cylinder_exact.npz` |
| time | about 25 s with 4 OpenMP threads (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the run to `runs/` and plots it):

   ```bash
   cd athenak-vet-gallery/cylinder
   jupyter nbconvert --to notebook --execute --inplace cylinder.ipynb
   ```

   or open it with `jupyter notebook cylinder.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` 30d0e3461 plus the tests of 3f2847099:

```
PASS idort_cylinder       22 s  E_in 0.000518 E_out 0.013 F_out 0.205
```

E follows exact transport to 1.3 % outside the cylinder; the moment flux in the vacuum outside
is about 20 % above the exact flux (`F_out`, a regression reference).

The input deck here is a copy for reading; the notebook runs the one in
`$ATHENAK/tests/vet_clean/decks/` (`run_tests.py` writes the deck it ran as `in.athinput` in
the run directory).
