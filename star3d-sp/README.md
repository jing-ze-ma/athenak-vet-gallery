# 3-D star test (sp): a blue supergiant envelope on a spherical-polar wedge

A small, complete 3-D star deck with the production scheme keys: spherical-polar wedge: r 20-80 Rsun, theta pi/3..2pi/3, phi 0..pi/3 (periodic lateral walls); the BSG production deck at 1/4 of its lateral resolution.
256 x 64 x 64 = 1.0 M cells, 16 MeshBlocks of 256 x 16 x 16; 4000 cycles, 9.8 d. It runs in about 15 minutes on 4 A100 GPUs; a few cycles on one CPU node show that the setup works.
Use it to check a new build or machine before a production run, and as a starting point for your own star decks.
Full text: the docs page [3-D stars](https://jing-ze-ma.github.io/athenak-vet-docs/stars.html).

| | |
|---|---|
| deck | `star3d_sp.athinput` (a copy of `$ATHENAK/inputs/rad_vet_clean/star3d_sp.athinput`) |
| data | `bsg_ic.txt`, `bsg_profile.txt`, `bsg_rosseland.txt`, `bsg_planck.txt` in `$ATHENAK/inputs/rad_vet_clean/star3d_data/` |
| notebook | `star3d.ipynb` (the same file as `$ATHENAK/docs/vet_clean/notebooks/star3d.ipynb`; it plots whichever of the sp and cs runs it finds) |

## Steps

1. Build `build_star/src/athena` (`-D PROBLEM=he_star_vet`): a CPU build for the start-up check and a
   CUDA or HIP build for the GPU run ([Build](https://jing-ze-ma.github.io/athenak-vet-docs/build.html), any block, the `build_star` lines).
2. Set up a run directory once, outside the checkout:

   ```bash
   export ATHENAK=/path/to/athenak
   mkdir -p star3d && cd star3d
   ln -s $ATHENAK/inputs/rad_vet_clean/star3d_data .
   cp $ATHENAK/inputs/rad_vet_clean/star3d_sp.athinput $ATHENAK/inputs/rad_vet_clean/star3d_cs.athinput .
   ```

3. One CPU node, does it start? One MPI rank per MeshBlock, one OpenMP thread per rank:

   ```bash
   export OMP_NUM_THREADS=1
   mpirun -np 16 $ATHENAK/build_star/src/athena -i star3d_sp.athinput -d run_sp_cpu time/nlim=3 > run_sp_cpu.log 2>&1
   ```

   Measured on one 2 x 56-core Intel Xeon Platinum 8480+ node: 3 cycles in 128 s (mostly start-up). It writes the
   t = 0 dumps; delete `run_sp_cpu` after.

4. 4 GPUs, the test run, one MPI rank per GPU (Slurm):

   ```bash
   srun -N 1 -n 4 --gpus-per-node=4 env KOKKOS_MAP_DEVICE_ID_BY=mpi_rank OMP_NUM_THREADS=1 \
     $ATHENAK/build_star/src/athena -i star3d_sp.athinput -d run_sp > run_sp.log 2>&1
   ```

   With `mpirun`: `mpirun -np 4 -x KOKKOS_MAP_DEVICE_ID_BY=mpi_rank -x OMP_NUM_THREADS=1 ...`.
   Over several A100 nodes with InfiniBand also set `UCX_IB_GPU_DIRECT_RDMA=0`.

5. Plot (numpy and matplotlib only):

   ```bash
   STAR3D_SP=run_sp jupyter nbconvert --to notebook --execute \
     $ATHENAK/docs/vet_clean/notebooks/star3d.ipynb --output star3d_run.ipynb --output-dir .
   ```

## What to expect

Measured on one node of 4 A100-40GB (MPCDF Raven, `gcc/13 cuda/12.6 openmpi_gpu/5.0`):

| deck | wall time | per cycle | simulated time | output |
|---|---|---|---|---|
| sp | 13.9 min (832 s) | 0.21 s | 4000 cycles, 9.8 d (dt about 260 s) | 6 dumps of hydro_w + vet, 0.6 GB |

Checks:

1. The return code is 0 and the log ends with the end-of-run summary.
2. `grep -c 'CLEAN WARNING' run_sp.log` gives 0.
3. The steady start converged (`clean_init steady: converged after N passes`); the line
   `clean_init steady pass 1: ... = ...` is a fingerprint of the binary: `9.025e-09` here (the same on CPU and A100).
4. Luminosity at the end of the 4-GPU run (`bsg3d.user.hst`, read columns by name):

| deck | L_in | L_top at the end | L_top / L_in at the end | L_top / L_in over the second half |
|---|---|---|---|---|
| sp | 4.651e37 (wedge) | 4.83e37 | 1.039 | 0.958 - 1.066 |

The committed `star3d.ipynb` was executed on the 4-A100 runs of both decks.
