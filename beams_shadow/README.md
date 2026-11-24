# Crossing beams and shadow

Two tests of the ray closure in optically thin regions (Ma et al. 2025, IDORT, sec. 4.1.1 and
4.1.4; Jiang 2021, ApJS 253, 49, sec. 4.2 and 4.3). Two beams cross in vacuum at ±45 deg; two
beams at ±15 deg pass an opaque ellipse and leave a shadow. The notebook plots E against the
exact straight and attenuated beams.

| | |
|---|---|
| property shown | closure accuracy (VET from rays): beams cross without interacting, sharp shadows |
| suite criterion | L1 of E (and the beam energy `lum_top`) within 5 % of the reference values; 4 MeshBlocks identical to 1 in tile mode |
| files | `beams_shadow.ipynb`, `idort_beams.athinput`, `idort_shadow.athinput`, `idort_shadow_gl.athinput` |
| time | about 60 s with 4 OpenMP threads per run (one CPU node) |

## Steps

1. Do the one-time setup of the [gallery README](../README.md#setup-once): a checkout of
   `athenak`, its CPU test binary `build_tests/src/athena`, and `ATHENAK` / `ATHENA_EXE`.
2. Run the notebook here (it runs the suite tests through `$ATHENAK/tests/vet_clean`,
   writes the runs to `runs/` and plots them):

   ```bash
   cd athenak-vet-gallery/beams_shadow
   jupyter nbconvert --to notebook --execute --inplace beams_shadow.ipynb
   ```

   or open it with `jupyter notebook beams_shadow.ipynb` and run all cells. A test whose
   record `runs/<test>.json` exists is loaded, not rerun; delete `runs/` to rerun.
3. Compare with the result below. The notebook prints the suite's PASS / FAIL line and the criterion.

## Result

The committed notebook was executed with a CPU build (GCC 13, `-ffp-contract=off`) of release
`vet-clean-release` 30d0e3461 plus the tests of 3f2847099:

```
PASS idort_beams          10 s  L1 0.777 lum_top 1.79 flux_err 2.66e-07
PASS idort_beams_mb       38 s  maxdiff_tile_x2 0 maxdiff_block_x1 2.05e-11
PASS idort_shadow          3 s  L1 0.0138
PASS idort_shadow_gl       5 s  L1 0.0126
```

The beams stay straight and 1-2 cells wide; an oblique beam narrower than a cell carries about
1.8 times the exact energy density at this resolution (`lum_top`), which dominates L1; the
energy flux through every face equals the injected flux to 3e-7 (`flux_err`).

The input deck(s) here are copies for reading; the notebook runs the ones in
`$ATHENAK/tests/vet_clean/decks/`, with the few keys the test changes on top
(`run_tests.py` writes the deck it ran as `in.athinput` in each run directory).
