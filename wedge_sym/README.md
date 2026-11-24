# Phi mirror symmetry on narrow wedges

The shell at rest and spherically symmetric, with the production Gauss-Legendre 7 x 18 ray set, 8 cycles: a 20 deg wedge (no key) and 3.7 and 7.3 deg wedges with `vet_gd_wall_sym = true`. The notebook plots F_phi per phi column and the wedge-mean F_phi of each wedge; the 3.7 deg wedge without the key must stop with the wedge FATAL.

| | |
|---|---|
| property shown | phi mirror symmetry |
| suite criterion | fphi = |sum F_phi| / sum |F_r| <= 1e-8; the run without the key stops with the FATAL |
| files | `wedge_sym.ipynb`, `shell_sp.athinput` |
| time | about 30 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/wedge_sym
   jupyter nbconvert --to notebook --execute --inplace wedge_sym.ipynb
   ```

   or open it with `jupyter notebook wedge_sym.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` 38f3d3c95:

```
PASS wedge_sym            22 s  fphi [1.58e-09 1.95e-11 1.87e-12] warnings 0 wedge_fatal 1
```

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
