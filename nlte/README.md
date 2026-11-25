# Two-level-atom non-LTE atmosphere

A scattering atmosphere with thermalisation parameter epsilon = 0.1 and 1e-4 (Jiang 2021, ApJS 253, 49, sec. 4.5). The notebook plots the mean intensity J against optical depth with the exact transport solution (the `nlte_exact_*.txt` files).

| | |
|---|---|
| property shown | closure accuracy (VET from rays) |
| suite criterion | L1 of J within 5 % of the reference value |
| files | `nlte.ipynb`, `nlte_eps1e-1.athinput`, `nlte_eps1e-4.athinput`, `nlte_exact_eps1e-1.txt`, `nlte_exact_eps1e-4.txt` |
| time | about 60 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/nlte
   jupyter nbconvert --to notebook --execute --inplace nlte.ipynb
   ```

   or open it with `jupyter notebook nlte.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS nlte_eps1e-1         17 s  L1 9.58e-05
PASS nlte_eps1e-4         17 s  L1 0.00133
```

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
