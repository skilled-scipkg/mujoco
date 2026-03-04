# mujoco source map: Advanced Topics

Use this map only after exhausting `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`
- `rg -n "mjv_|mjr_|mj_step|mj_loadXML|schema|save" include plugin simulate src python/mujoco`

## Function-level entry points
### Visualization and output internals
- `simulate/main.cc` | CLI startup and model load path into simulation UI.
- `simulate/simulate.cc` | Viewer loop, stepping, and scene update flow.
- `src/engine/engine_vis_visualize.c` | Scene construction and visualization update functions.
- `src/engine/engine_vis_interact.c` | Camera perturbation and interaction handlers.
- `include/mujoco/mjvisualize.h` | Public visualization API declarations.
- `python/mujoco/viewer.py` | Python viewer launch and update behavior.
- `sample/record.cc` | Example output/recording path.

### XML schema and save/load behavior
- `src/xml/xml_native_reader.cc` | MJCF parse/compile path.
- `src/xml/xml_native_writer.cc` | XML serialization and round-trip writing.
- `src/xml/xml_api.cc` | XML API surface used by parser/serializer wrappers.
- `src/engine/engine_io.c` | Engine-side print/save IO utilities.
- `sample/compile.cc` | Compile and save pipeline example.

## Behavior-check commands
- `cmake -S . -B build -DCMAKE_BUILD_TYPE=Release && cmake --build build -j`
- `./build/simulate/simulate model/humanoid/humanoid.xml`
- `./build/sample/compile model/humanoid/humanoid.xml build/humanoid_compiled.xml`
