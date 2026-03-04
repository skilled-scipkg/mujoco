# mujoco documentation map: MJX

Use this map for accelerator-backed MuJoCo execution (`jax` and `warp`) and batching/performance tradeoffs.

## Core references
- `doc/mjx.rst` | MJX overview, setup, and behavior notes.
- `doc/mjx_api.rst` | MJX API details.
- `doc/mjwarp/index.rst` | Warp backend overview.
- `doc/mjwarp/api.rst` | Warp API and runtime expectations.
- `mjx/README.md` | Package usage and examples.
- `mjx/cuda_requirements.txt` | CUDA-side requirements for GPU workflows.

## Runnable references
- `mjx/mujoco/mjx/test_data/humanoid/README.md`
- `mjx/mujoco/mjx/test_data/barkour_v0/README.md`
- `mjx/mujoco/mjx/_src/io_test.py`
- `mjx/mujoco/mjx/_src/forward_test.py`

## Escalation
- Use `references/source_map.md` for function-level stepping/collision/solver checks.
