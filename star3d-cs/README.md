# 3-D star test (cs): a red supergiant on a cubed sphere

A small, complete 3-D star deck with the production scheme keys: cubed sphere, full sphere: r 25-2530 Rsun (stretched; photosphere at 834 Rsun), 6 faces x 32 x 32; a 20.5 Msun RSG (MESA profile), the RSG production deck at 1/4 of its lateral resolution.
548 x 32 x 32 x 6 = 3.4 M cells, 24 MeshBlocks of 548 x 16 x 16; 1200 cycles, 13.9 d. It runs in about 12 minutes on 4 MI300A APUs; a few cycles on one CPU node show that the setup works.
Use it to check a new build or machine before a production run, and as a starting point for your own star decks.
Full text: the docs page [3-D stars](https://jing-ze-ma.github.io/athenak-vet-docs/stars.html).

| | |
|---|---|
| deck | `star3d_cs.athinput` (a copy of `$ATHENAK/inputs/stars/star3d_cs.athinput`) |
| data | `rsg_ic.txt`, `rsg_rosseland.txt`, `rsg_planck.txt` in `$ATHENAK/inputs/stars/star3d_data/` |
| notebook | `star3d.ipynb` (the same file as `$ATHENAK/docs/vet_clean/notebooks/star3d.ipynb`; it plots whichever of the sp and cs runs it finds) |

## Steps

1. Build `build_star/src/athena` (`-D PROBLEM=star_vet`): a CPU build for the start-up check and a
   CUDA or HIP build for the GPU run ([Build](https://jing-ze-ma.github.io/athenak-vet-docs/build.html), any block, the `build_star` lines).
2. Set up a run directory once, outside the checkout:

   ```bash
   export ATHENAK=/path/to/athenak
   mkdir -p star3d && cd star3d
   ln -s $ATHENAK/inputs/stars/star3d_data .
   cp $ATHENAK/inputs/stars/star3d_sp.athinput $ATHENAK/inputs/stars/star3d_cs.athinput .
   ```

3. One CPU node, does it start? One MPI rank per MeshBlock, one OpenMP thread per rank:

   ```bash
   export OMP_NUM_THREADS=1
   mpirun -np 24 $ATHENAK/build_star/src/athena -i star3d_cs.athinput -d run_cs_cpu time/nlim=2 > run_cs_cpu.log 2>&1
   ```

   Measured on one 2 x 56-core Intel Xeon Platinum 8480+ node: 2 cycles in 125 s (mostly start-up). It writes the
   t = 0 dumps; delete `run_cs_cpu` after.

4. 4 GPUs, the test run, one MPI rank per GPU (Slurm):

   ```bash
   srun -N 1 -n 4 --gpus-per-node=4 env KOKKOS_MAP_DEVICE_ID_BY=mpi_rank OMP_NUM_THREADS=1 \
     $ATHENAK/build_star/src/athena -i star3d_cs.athinput -d run_cs > run_cs.log 2>&1
   ```

   With `mpirun`: `mpirun -np 4 -x KOKKOS_MAP_DEVICE_ID_BY=mpi_rank -x OMP_NUM_THREADS=1 ...`.
   Over several A100 nodes with InfiniBand also set `UCX_IB_GPU_DIRECT_RDMA=0`.

5. Plot (numpy and matplotlib only):

   ```bash
   STAR3D_CS=run_cs jupyter nbconvert --to notebook --execute \
     $ATHENAK/docs/vet_clean/notebooks/star3d.ipynb --output star3d_run.ipynb --output-dir .
   ```

## What to expect

Measured on 4 MI300A APUs (two nodes of MPCDF Viper, `gcc/16 rocm/7.2 openmpi_gpu/5.0`):

| deck | wall time | per cycle | simulated time | output |
|---|---|---|---|---|
| cs | 11.9 min (713 s) | 0.59 s | 1200 cycles, 13.9 d (dt 1000 s) | 6 dumps of hydro_w + vet, 1.9 GB |

Checks:

1. The return code is 0 and the log ends with the end-of-run summary.
2. `grep -c 'CLEAN WARNING' run_cs.log` gives 0.
3. The steady start converged (`clean_init steady: converged after N passes`); the line
   `clean_init steady pass 1: ... = ...` is a fingerprint of the binary: `1.140e-07` here (the same on CPU and MI300A).
4. Luminosity at the end of the 4-GPU run (`rsg.user.hst`, read columns by name):

| deck | L_in | L_top at the end | L_top / L_in at the end | L_top / L_in over the second half |
|---|---|---|---|---|
| cs (at 13.9 d; falling: the mixing-length scaffold is ramped down over 105 d, see the docs page) | 6.837e38 | 6.74e38 | 0.986 | 0.986 - 0.997 |

The committed `star3d.ipynb` was executed on the 4-MI300A runs of both decks.
