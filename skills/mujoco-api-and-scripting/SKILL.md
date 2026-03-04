---
name: mujoco-api-and-scripting
description: This skill should be used when users ask about api and scripting in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: API and Scripting

## High-Signal Playbook

### Route Conditions
- Use this skill for C API usage, Python bindings usage, and API lookup/navigation.
- Route accelerator-first JAX/Warp execution and tuning to `mujoco-mjx`.
- Route OpenUSD import/export workflow specifics to `mujoco-openusd`.
- Route installation/build errors to `mujoco-build-and-install`.

### Triage Questions
- Are you calling C API directly, Python bindings, MJX, or MuJoCo Warp?
- Do you need model load/step loop, callbacks, rendering, model editing, or rollout utilities?
- Is the question about API parity/mapping between C and Python names/signatures?
- Do you need a notebook-backed example (Python tutorial, mjspec, rollout, MJX)?
- Are failures parser/compile (`mj_loadXML`/`mj_compile`) or runtime (`mj_step`/callbacks)?
- Do you need behavior confirmation via tests/examples before writing new code?

### Canonical Workflow
1. Start in `doc/APIreference/index.rst` to locate the right category (`APItypes`, `APIfunctions`, `APIglobals`).
2. Map C-to-Python expectations using `doc/python.rst` (same core API, Python-specific deviations documented).
3. Build minimal loop: load model -> create data -> step -> inspect state.
4. For Python convenience, use named access and `mj_step(..., nstep=...)` where suitable. (Ref: `doc/python.rst`)
5. For device execution, use MJX: `mjx.put_model`/`mjx.put_data`/`mjx.make_data` + `jax.jit`. (Ref: `doc/mjx.rst`)
6. Confirm behavior against runnable examples/tests before extending.
7. Only if docs are insufficient, inspect binding/source entry points (`python/mujoco`, `include/mujoco`, `mjx/mujoco/mjx`).

### Minimal Working Example
```python
import mujoco
from mujoco import mjx

m = mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml')
d = mujoco.MjData(m)
mujoco.mj_step(m, d, nstep=5)

mx = mjx.put_model(m)
dx = mjx.put_data(m, d)
dx = mjx.step(mx, dx)
print('cpu_time', d.time, 'mjx_qpos0', float(dx.qpos[0]))
```

### Pitfalls and Fixes
- Assuming Python bindings are fully "Pythonic": argument ordering mostly mirrors C API. (Ref: `doc/python.rst`)
- Forgetting that many arrays are views into live engine memory: use `.copy()` for logs/history. (Ref: `doc/python.rst`)
- Expecting manual size arguments in Python wrappers for dynamic arrays: sizes are inferred. (Ref: `doc/python.rst`)
- Using Python callbacks in performance-critical loops: GIL transitions can dominate; move hot callbacks native where possible. (Ref: `doc/python.rst`)
- Mixing `mj_step1`/`mj_step2` with RK4 controller assumptions: RK4 requires callback path. (Ref: `doc/programming/simulation.rst`)
- Treating MJX feature set as identical to MuJoCo C/Python for all features: confirm parity notes first. (Ref: `doc/mjx.rst`)

### Convergence and Validation Checks
- Minimal load/step loop runs without exceptions in selected API surface.
- C/Python symbol mapping verified against API reference and `doc/python.rst` notes.
- For MJX path, one jitted step executes with expected tensor shapes/dtypes.
- Behavioral sanity: short MJX rollout is qualitatively consistent with CPU baseline for same initial state.

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/APIreference/index.rst`
- `doc/APIreference/APIfunctions.rst`
- `doc/APIreference/APIglobals.rst`
- `doc/APIreference/functions_override.rst`
- `doc/python.rst`
- `doc/mjx.rst`
- `doc/mjx_api.rst`
- `doc/mjwarp/index.rst`
- `doc/mjwarp/api.rst`

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
- `include/mujoco/mujoco.h`
- `python/mujoco/functions.h`
- `python/mujoco/functions.cc`
- `python/mujoco/introspect/functions.py`
- `python/mujoco/introspect/codegen/generate_functions.py`
- `python/mujoco/viewer.py`
- `python/mujoco/rollout.cc`
- `python/mujoco/usd/exporter.py`
- `mjx/mujoco/mjx/_src/io.py`
- `mjx/mujoco/mjx/_src/forward.py`
- `mjx/mujoco/mjx/_src/solver.py`
- `mjx/mujoco/mjx/warp/forward.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
