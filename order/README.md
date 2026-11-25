# Order of accuracy in time and space

`order_time`: a spherical shell with kinematic gas and Marshak walls; the time step is halved on a fixed grid. `order_space`: a linear radiation-hydro wave, one period, N = 16 ... 128 cells. `order_stiff`: a trapped radiation-acoustic wave, the time step halved. The notebook plots the L1 error against dt, N and the steps per period, with a slope-2 line.

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
   cd athenak-vet-notebooks/order
   jupyter nbconvert --to notebook --execute --inplace order.ipynb
   ```

   or open it with `jupyter notebook order.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS order_time           27 s  err_E [0.00102 0.000344 0.000113 3.15e-05] order_E [1.57 1.61 1.84] err_F [0.00475 0.00168 0.000588 0.000168] order_F [1.5 1.52 1.81] err_T [0.000167 6.61e-05 2.46e-05 6.91e-06] order_T [1.34 1.43 1.83]
PASS order_stiff          11 s  err_E [4.31e-06 1.13e-06 2.9e-07] order_E [1.94 1.96] err_v [0.00449 0.00118 0.000305] order_v [1.92 1.96]
PASS order_space         236 s  err_E [0.0907 0.0208 0.00434] order_E [2.12 2.26] err_v [0.106 0.0379 0.00979] order_v [1.48 1.95]
```

Deck D is pre-asymptotic at these time steps (successive orders 1.57, 1.61, 1.84 for E); the criterion is on the finest pair.

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
