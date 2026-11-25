# Conservation on a closed rotating shell

A rotating gas + radiation shell in a closed box (reflecting walls), 3-D spherical polar (40 cycles) and cubed sphere (12 cycles). The scheme conserves total energy (gas + radiation), mass and total L_z; the notebook plots their drift against cycle.

| | |
|---|---|
| property shown | energy, mass, angular momentum |
| suite criterion | dE/E, dM/M, dLz <= 1e-13 |
| files | `shell.ipynb`, `shell_sp.athinput`, `shell_cs.athinput` |
| time | about 30 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-notebooks/conservation
   jupyter nbconvert --to notebook --execute --inplace shell.ipynb
   ```

   or open it with `jupyter notebook shell.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS conservation_sp       5 s  dE 1e-15 dM 6.74e-16 dLz 1.47e-16 dLz_gr 5.92e-07
PASS conservation_cs       6 s  dE 1.32e-15 dM 2.61e-16 dLz 1.17e-16 dLz_gr 6.48e-05
```

`dLz` is the drift of `Lz_tot` (gas + radiation + the L_z ledger); `dLz_gr` is the drift of gas + radiation alone (on spherical polar the face-flux cone cut of the first cycles; on the cubed sphere a diagnostic mismatch, not the scheme's L_z). See the docs page Properties.

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
