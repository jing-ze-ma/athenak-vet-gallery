# Restart is bitwise

`restart_sp`: the shell run 16 cycles straight, and 8 cycles + a restart dump + 8 more cycles. `restart_cs`: the cubed sphere, 8 straight vs 4 + 4. The notebook plots the difference.

| | |
|---|---|
| property shown | restart, dt retry |
| suite criterion | every dump and history row identical (difference 0) |
| files | `restart.ipynb`, `shell_sp.athinput`, `shell_cs.athinput` |
| time | about 40 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/restart
   jupyter nbconvert --to notebook --execute --inplace restart.ipynb
   ```

   or open it with `jupyter notebook restart.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS restart_sp            8 s  files 4 maxdiff 0
PASS restart_cs            9 s  files 4 maxdiff 0
```

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
