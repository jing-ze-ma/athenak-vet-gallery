# Well-balancing: an equilibrium envelope stays at rest

A grey scattering envelope in hydrostatic and radiative equilibrium (2-D spherical polar to t = 10; cubed sphere to t = 2.5) must stay at rest. The notebook plots max |v|/c_s against time and the radial profiles of the spherical-polar run.

| | |
|---|---|
| property shown | well-balance |
| suite criterion | max over the run of max |v|/c_s <= 1e-8 |
| files | `wb.ipynb`, `rhse_sp.athinput`, `rhse_cs.athinput` |
| time | about 110 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/well_balance
   jupyter nbconvert --to notebook --execute --inplace wb.ipynb
   ```

   or open it with `jupyter notebook wb.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` 38f3d3c95:

```
PASS wb_sp                10 s  vmax_over_cs 4.57e-09
PASS wb_cs                83 s  vmax_over_cs 1.18e-09
```

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
