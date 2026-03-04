---
name: mujoco-index
description: This skill should be used when users ask how to use mujoco and the correct generated documentation skill must be selected before going deeper into source code.
---

# mujoco Skills Index

## Route the Request
- Triage first by intent: install/build, getting-started, modeling inputs, simulation/runtime behavior, API/scripting, MJX, OpenUSD, or narrow advanced topics.
- Keep responses docs-first; do not jump to source until topic docs and examples are exhausted.

## Intent Triage Matrix
- `Install/build/runtime setup failures`: `mujoco-build-and-install`
- `First run / quickstart / branch selection`: `mujoco-getting-started`
- `MJCF/URDF authoring, schema semantics, model composition`: `mujoco-inputs-and-modeling`
- `C/Python bindings, API lookup, scripting patterns`: `mujoco-api-and-scripting`
- `JAX/Warp accelerated simulation`: `mujoco-mjx`
- `USD import/export and USD-enabled builds`: `mujoco-openusd`
- `Narrow topics (changelog/xmlschema/visualization/static examples/docs-index)`: `mujoco-advanced-topics`
- `Architecture/developer internals`: `mujoco-developer-guide`
- `Two-doc auxiliary topic`: `mujoco-includes`

## Generated Topic Skills
- `mujoco-build-and-install`
- `mujoco-inputs-and-modeling`
- `mujoco-getting-started`
- `mujoco-api-and-scripting`
- `mujoco-mjx`
- `mujoco-openusd`
- `mujoco-advanced-topics`
- `mujoco-developer-guide`
- `mujoco-includes`

## Docs-First Escalation Policy
1. Start with the selected skill's "Primary documentation references".
2. If unclear, inspect `skills/<skill-id>/references/doc_map.md`.
3. Use runnable references (notebooks/examples/tests) before inferring behavior.
4. Only then inspect `skills/<skill-id>/references/source_map.md` and concrete source entry points.
5. Use targeted symbol search while inspecting source:
   `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`

## Fast Start Handoff (Real Simulation)
1. Build once: `cmake -S . -B build -DCMAKE_BUILD_TYPE=Release && cmake --build build -j`.
2. Native smoke run: `./build/simulate/simulate model/humanoid/humanoid.xml`.
3. Python smoke run:
   `python -c "import mujoco; m=mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml'); d=mujoco.MjData(m); mujoco.mj_step(m,d); print(d.time)"`.
4. Route to topic skill immediately after this checkpoint.

## Runnable Reference Roots
- Notebooks/tutorial assets: `python`, `mjx`
- Examples/samples: `sample`, `simulate`, `model`
- Tests/behavior checks: `test`, `python/mujoco`, `mjx/mujoco/mjx`

## Source Directories for Deep Inspection
- `include`
- `plugin`
- `simulate`
- `src`
- `python/mujoco`
- `wasm/codegen`
- `mjx/mujoco/mjx`
