# Order of accuracy in time and space

`order_time`: a spherical shell with kinematic gas and Marshak walls; the time step is halved on a fixed grid. `order_space`: a linear radiation-hydro wave, one period, N = 16 ... 128 cells. The notebook plots the L1 error against dt and N, with a slope-2 line.

| | |
|---|---|
| property shown | order |
| suite criterion | order of the finest pair >= 1.7; errors within 5 % of the reference |
| files | `order.ipynb`, `deckD.athinput`, `wave_cart.athinput` |
| time | about 230 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/order
   jupyter nbconvert --to notebook --execute --inplace order.ipynb
   ```

   or open it with `jupyter notebook order.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` 38f3d3c95:

```
PASS order_time           30 s  err_E [0.00102 0.000344 0.000113 3.15e-05] order_E [1.57 1.61 1.84] err_F [0.00477 0.00169 0.000589 0.000168] order_F [1.5 1.52 1.81] err_T [0.000167 6.61e-05 2.45e-05 6.9e-06] order_T [1.34 1.43 1.83]
PASS order_space         188 s  err_E [0.071 0.0205 0.00449] order_E [1.79 2.19] err_v [0.12 0.0424 0.0111] order_v [1.5 1.94]
```

Deck D is pre-asymptotic at these time steps (successive orders 1.57, 1.61, 1.84 for E); the criterion is on the finest pair.

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
