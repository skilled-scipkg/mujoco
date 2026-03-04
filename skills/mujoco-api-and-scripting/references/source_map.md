# mujoco source map: API and Scripting

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`
- `rg -n "mj_step|mj_forward|mj_loadXML|MjModel|MjData|callback|viewer" include src python/mujoco`

## Function-level entry points
### C API and core simulation behavior
- `include/mujoco/mujoco.h` | Public C API declarations.
- `include/mujoco/mjspec.h` | Programmatic model-editing API surface.
- `src/engine/engine_forward.c` | Forward dynamics and stepping internals.
- `src/engine/engine_callback.c` | Callback dispatch integration.
- `src/user/user_api.cc` | `mjs_*` behavior backing model-edit APIs.

### Python bindings and runtime wrappers
- `python/mujoco/functions.h` | Binding declarations and signature glue.
- `python/mujoco/functions.cc` | Core Python <-> C function wrappers.
- `python/mujoco/callbacks.cc` | Python callback bridging.
- `python/mujoco/viewer.py` | Viewer APIs and event loop usage.
- `python/mujoco/rollout.cc` | High-throughput rollout helper implementation.
- `python/mujoco/introspect/functions.py` | Introspection metadata for bindings.
- `python/mujoco/introspect/codegen/generate_functions.py` | Binding generation pipeline.

### MJX/JAX and Warp API implementation
- `mjx/mujoco/mjx/_src/io.py` | Model/data transfer between MuJoCo and MJX.
- `mjx/mujoco/mjx/_src/forward.py` | MJX forward-step path.
- `mjx/mujoco/mjx/_src/solver.py` | Constraint/solver internals.
- `mjx/mujoco/mjx/warp/io.py` | Warp-side IO adaptation.
- `mjx/mujoco/mjx/warp/forward.py` | Warp forward-step behavior.
- `mjx/mujoco/mjx/warp/ffi.py` | Warp FFI and graph execution control.

### Behavior/regression checks
- `python/mujoco/bindings_test.py`
- `python/mujoco/usd/exporter_test.py`
- `mjx/mujoco/mjx/_src/io_test.py`
- `mjx/mujoco/mjx/_src/forward_test.py`

## Behavior-check commands
- `python -c "import mujoco; m=mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml'); d=mujoco.MjData(m); mujoco.mj_step(m,d); print(d.time)"`
- `python -m pytest python/mujoco/bindings_test.py -k step`
- `python -m pytest mjx/mujoco/mjx/_src/forward_test.py -k step`
