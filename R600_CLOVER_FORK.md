# Experimental r600 Clover compatibility branch

This branch retains Mesa **25.0.7** for legacy Radeon OpenCL training experiments.
Its base is upstream tag `mesa-25.0.7`, commit
`742a20f48c59e8649533c84c4d49dd95b403f5da`. Mesa's original project identity,
history and licenses are retained. This is not an official Mesa release or a
replacement recommendation for a working system display driver.

The branch contains the tested r600 changes for:

- Ordered global-pool placement and first-fitting-hole allocation.
- Opt-in cache flushing before relevant CP DMA copies
  (`R600_CP_DMA_CB_FLUSH=1` in the tested profile).
- Optional upfront pool reservation (`R600_POOL_RESERVE_MB`, 1..1024 MiB),
  with fallback/checking for initial reservation failure. A request is not
  measured capacity; later pool-growth/OOM paths still have limitations.

The runtime was tested with patched DLPrimitives and the pytorch_dlprim extension
on a 2 GiB HD6970: 960 MiB reserve, batch 24, 256px FP32, 30 completed baseline
epochs. No reserve is yet recommended for 1 GiB HD68xx/HD58xx hardware. Do not
copy the 960 MiB setting to those cards. See the backend fork's
`docs/r600-clover.md` for the full profile, build options, tests and limitations.

Consumers should pin an exact commit of this branch and build an isolated Clover
runtime. Do not follow Mesa main for this dependency: Clover was removed there
in the 25.2 release series. This branch is preserved while newer Rusticl support
and possible upstream fixes are investigated separately; it does not claim
equivalent support on every r600-family GPU.
