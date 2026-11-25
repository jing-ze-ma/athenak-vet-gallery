# Floors with a general EOS, paid from the radiation

The closed shell with `hydro/eos = general` and a temperature floor just above the gas temperature, so the floor raises T in most cells every cycle; `floor_pay = rad_column` pays that energy from the radiation. The notebook plots the total-energy drift from cycle 1 on and the floor energy paid per cycle.

| | |
|---|---|
| property shown | energy with floors; positivity |
| suite criterion | dE <= 1e-13 from cycle 1 on; floor energy paid > 0, unpaid 0; no `CLEAN WARNING` |
| files | `floor_pay.ipynb`, `shell_sp.athinput` |
| time | about 20 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite test through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-notebooks/floor_pay
   jupyter nbconvert --to notebook --execute --inplace floor_pay.ipynb
   ```

   or open it with `jupyter notebook floor_pay.ipynb` and run all cells. A test whose record
   `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` ccab558ee:

```
PASS floor_pay            27 s  dE 8.34e-16 dE0 5e-16 paid 0.153 unpaid 0 warnings 0 worst_lines 185 worst_bitwise True ekin_floor_dE 1e-15 ekin_floor_dM 6.74e-16 ekin_cart_dM 2.22e-16 ekin_dE 8.34e-16 ekin_dM 5.4e-16 ekin_dLz 1.47e-16
```

`dE0` is the payment of the initial conversion at the top of cycle 1, after the t = 0 output.

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
