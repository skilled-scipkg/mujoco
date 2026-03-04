# mujoco source map: Getting Started

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" simulate sample src/xml src/engine python/mujoco mjx/mujoco/mjx`
- `rg -n "mj_step|mj_loadXML|MjModel|MjData|viewer|rollout" simulate sample src python/mujoco`

## Function-level entry points
### Native startup and stepping path
- `simulate/main.cc` | CLI startup and model loading.
- `simulate/simulate.cc` | Main interactive simulation loop.
- `sample/basic.cc` | Minimal load+step executable.
- `sample/compile.cc` | Compile-time model validation example.
- `sample/testspeed.cc` | Throughput benchmark stepping path.

### Parser/compiler and runtime core
- `src/xml/xml_api.cc` | Model parse/compile API wrappers.
- `src/xml/xml_native_reader.cc` | MJCF/URDF read pipeline.
- `src/engine/engine_forward.c` | Core stepping dynamics.
- `include/mujoco/mujoco.h` | Public API used in smoke snippets.

### Python and MJX onboarding implementations
- `python/mujoco/viewer.py` | Python viewer entry path.
- `python/mujoco/rollout.cc` | Rollout utility implementation.
- `python/mujoco/bindings_test.py` | Baseline Python behavior checks.
- `mjx/mujoco/mjx/_src/io.py` | CPU model/data to MJX transfer.
- `mjx/mujoco/mjx/viewer.py` | MJX viewer tooling entry.

## Behavior-check commands
- `./build/simulate/simulate model/humanoid/humanoid.xml`
- `python -c "import mujoco; m=mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml'); d=mujoco.MjData(m); [mujoco.mj_step(m,d) for _ in range(10)]; print(d.time)"`
