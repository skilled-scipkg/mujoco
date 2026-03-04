---
name: mujoco-mjx
description: This skill should be used when users ask about mjx in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: Mjx

## High-Signal Playbook

### Route Conditions
- Use this skill for JAX/Warp MuJoCo execution (`mujoco-mjx` package), batching, and accelerator tuning.
- Route base C/Python API questions to `mujoco-api-and-scripting`.
- Route install/build environment failures (toolchain, package setup) to `mujoco-build-and-install`.
- Route OpenUSD import/export questions to `mujoco-openusd`.

### Triage Questions
- Which implementation do you need: MJX-JAX (`impl='jax'` default) or MJX-Warp (`impl='warp'`)? (Ref: `doc/mjx.rst`)
- What hardware/backend is targeted (CPU/GPU/TPU, NVIDIA-specific Warp)?
- Do you need autodiff? (MJX-Warp currently does not provide it; Ref: `doc/mjx.rst`)
- What batch size and rollout shape are required (`vmap`, `jit`, `pmap`)?
- Are contact/constraint buffers (`naconmax`, `njmax`) sized for your workload in Warp mode?
- Are you seeing graph recapture overhead in Warp FFI (`graph_mode` tuning needed)?

### Canonical Workflow
1. Install `mujoco-mjx` (and `[warp]` extras if needed). (Ref: `doc/mjx.rst`)
2. Compile/load model with core MuJoCo first, then place model/data on device (`mjx.put_model`, `mjx.put_data`/`mjx.make_data`). (Ref: `doc/mjx.rst`)
3. Build minimal step function and JIT it before scaling batch size.
4. Add batching via `jax.vmap` only after single-env correctness is verified.
5. For Warp mode, set `impl='warp'` and tune `naconmax`/`njmax`; monitor overflow behavior. (Ref: `doc/mjx.rst`)
6. If CUDA graph captures are unstable, switch to staged graph modes (`WARP_STAGED`/`WARP_STAGED_EX`). (Ref: `doc/mjx.rst`)
7. Use `mjx-testspeed`/`mjx-viewer` and relevant tests for performance and parity checks. (Ref: `doc/mjx.rst`)

### Minimal Working Example
```python
import jax
import jax.numpy as jnp
import mujoco
from mujoco import mjx

m = mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml')
mx = mjx.put_model(m)

@jax.jit
def step_once(dx):
  return mjx.step(mx, dx)

dx = mjx.make_data(mx)
dx = step_once(dx)
print('qpos0', float(dx.qpos[0]))

# Batch sketch
# batched = jax.vmap(lambda v: mjx.step(mx, mjx.make_data(mx).replace(qvel=v)))
```

### Pitfalls and Fixes
- Expecting full feature parity with MuJoCo C/Python in all cases: check parity notes first. (Ref: `doc/mjx.rst`)
- Using Warp when autodiff is required: choose MJX-JAX path for gradient workflows. (Ref: `doc/mjx.rst`)
- Under-sizing Warp contact/constraint budgets (`naconmax`, `njmax`): increase based on observed overflows. (Ref: `doc/mjx.rst`)
- Reading contacts from deprecated locations: for Warp, prefer supported sensor paths; contact internals moved. (Ref: `doc/mjx.rst`)
- Batch rendering misuse: `nworld` fixed at render-context creation; shape mismatches otherwise. (Ref: `doc/mjx.rst`)

### Convergence and Validation Checks
- Single-env jitted stepping works with stable shapes/dtypes.
- Short MJX rollout is numerically sane (no NaNs/divergence) and qualitatively matches CPU baseline.
- Warp mode runs without repeated buffer overflow symptoms.
- Throughput improves (or at least does not regress badly) when moving from eager to jitted/batched execution.

## Scope
- Handle questions about documentation grouped under the 'mjx' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/mjx.rst`
- `doc/mjx_api.rst`
- `mjx/README.md`
- `mjx/mujoco/mjx/test_data/barkour_v0/README.md`
- `mjx/mujoco/mjx/test_data/humanoid/README.md`
- `mjx/cuda_requirements.txt`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `mjx`
- `model`
- `python`
- `sample`
- `wasm/demo_app`

## Test references
- `test`
- `python/mujoco`
- `unity/Tests`
- `wasm/tests`
- `mjx/mujoco/mjx`

## Optional deeper inspection
- `include`
- `plugin`
- `simulate`
- `src`
- `python/mujoco`
- `wasm/codegen`
- `mjx/mujoco/mjx`

## Source entry points for unresolved issues
- `mjx/mujoco/mjx/_src/io.py`
- `mjx/mujoco/mjx/_src/forward.py`
- `mjx/mujoco/mjx/_src/solver.py`
- `mjx/mujoco/mjx/_src/dataclasses.py`
- `mjx/mujoco/mjx/_src/collision_driver.py`
- `mjx/mujoco/mjx/warp/forward.py`
- `mjx/mujoco/mjx/warp/ffi.py`
- `mjx/mujoco/mjx/warp/render_context.py`
- `mjx/mujoco/mjx/warp/collision_driver.py`
- `mjx/mujoco/mjx/viewer.py`
- `mjx/mujoco/mjx/warp/visualize_render.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
