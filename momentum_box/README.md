# Linear momentum in a periodic box (a report)

A periodic 3-D Cartesian box (8^3) with a radiation wave along the xyz diagonal, amplitude 1e-3, 8 cycles. Total linear momentum = gas + radiation face momenta. The notebook prints and plots the drift per cycle.

| | |
|---|---|
| property shown | linear momentum (not conserved in general) |
| suite criterion | a report, not a criterion: passes when the run completes, the numbers are finite and there is no `CLEAN WARNING` |
| files | `momentum_box.ipynb`, `wave_cart.athinput` |
| time | about 20 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-notebooks/momentum_box
   jupyter nbconvert --to notebook --execute --inplace momentum_box.ipynb
   ```

   or open it with `jupyter notebook momentum_box.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS momentum_box          7 s  dP_max 8.89e-17 dP_cyc [8.4e-17 8.25e-17 8.16e-17 8.28e-17 8.6e-17 8.16e-17 8.25e-17 8.89e-17] warnings 0
```

At this amplitude none of the terms that change linear momentum acts, so the drift is round-off; the scheme does not conserve linear momentum in general (docs page Properties).

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
