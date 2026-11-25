# Thermal equilibrium

Uniform gas and radiation out of equilibrium relax to a common temperature at constant total energy (Jiang 2021, ApJS 253, 49, sec. 4.1). The notebook plots E(t), T(t) against the exact ODE, their errors and the total-energy drift.

| | |
|---|---|
| property shown | energy; order of the exchange |
| suite criterion | max relative error of E(t), T(t) <= bound; total-energy drift <= 1e-13 |
| files | `thermal_eq.ipynb`, `thermal_eq.athinput` |
| time | about 40 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/thermal_eq
   jupyter nbconvert --to notebook --execute --inplace thermal_eq.ipynb
   ```

   or open it with `jupyter notebook thermal_eq.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS thermal_eq           25 s  errT_h 0.000552 dEtot_h 1.12e-15 errT_h2 0.000124 dEtot_h2 7e-16 order 2.16
```

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
