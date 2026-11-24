# Radiation-acoustic waves in every trapping regime

A small (1e-6) sound wave in a uniform periodic gas + radiation box (64 x 4 cells, two periods, cfl 0.3), started on the exact eigenmode, at grid-scale trapping parameters x = 0.01, 0.1, 1 and 100. The phase speed and damping of the k = 1 mode are fitted and compared with the analytic root of the linear gas + moment system.

| | |
|---|---|
| property shown | radiation-acoustic waves |
| suite criterion | phase speed within 3e-3, damping within 5e-3 per radian, no growth; dt gain of `rad_signal_speed_diff` within 2 % of 2.77 |
| files | `radwave.ipynb`, `wave_cart.athinput` |
| time | about 60 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/radwave
   jupyter nbconvert --to notebook --execute --inplace radwave.ipynb
   ```

   or open it with `jupyter notebook radwave.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` 38f3d3c95:

```
PASS radwave              57 s  err_cph [0.000389 0.00188 0.00137 0.000368] err_damp [0.00187 7.1e-05 0.000173 0.000679] damp [0.715 0.164 0.0152 0.000773] warnings 0 dt_gain 2.77
```

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
