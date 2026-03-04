# mujoco source map: OpenUSD

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" src/experimental/usd plugin/usd_decoder python/mujoco/usd test/experimental/usd`
- `rg -n "USD|SdfFileFormat|mjcPhysics|export|import|decoder" src/experimental/usd plugin/usd_decoder python/mujoco/usd`

## Function-level entry points
### Build and shared USD plumbing
- `src/experimental/usd/CMakeLists.txt` | USD feature build integration.
- `src/experimental/usd/writer.cc` | Core USD writing pipeline.
- `src/experimental/usd/utils.cc` | Shared USD utility behavior.
- `src/experimental/usd/layer_sink.cc` | Layer output sink behavior.
- `include/mujoco/experimental/usd/writer.h` | Writer API declarations.
- `include/mujoco/experimental/usd/utils.h` | Utility API declarations.

### MJCF file-format plugin and conversion path
- `src/experimental/usd/plugins/mjcf/mjcf_file_format.cc` | USD plugin entry for MJCF support.
- `src/experimental/usd/plugins/mjcf/mujoco_to_usd.cc` | MJCF -> USD conversion implementation.
- `src/experimental/usd/plugins/mjcf/utils.cc` | Conversion helpers.
- `src/experimental/usd/plugins/mjcf/plugInfo.json` | Plugin registration metadata.

### USD decoder and schema behavior
- `plugin/usd_decoder/usd_decoder.cc` | Runtime USD decode path.
- `plugin/usd_decoder/material_parsing.cc` | Material parsing behavior.
- `plugin/usd_decoder/kinematic_tree.cc` | USD kinematic hierarchy mapping.
- `src/experimental/usd/mjcPhysics/sceneAPI.cpp` | Scene schema behavior.
- `src/experimental/usd/mjcPhysics/materialAPI.cpp` | Material schema behavior.

### Python exporter path and tests
- `python/mujoco/usd/exporter.py` | Exporter implementation.
- `python/mujoco/usd/demo.py` | End-to-end exporter usage.
- `python/mujoco/usd/exporter_test.py` | Exporter behavior checks.
- `test/experimental/usd/plugins/mjcf/mjcf_file_format_test.cc` | Plugin behavior test.
- `test/experimental/usd/mjcPhysics/mjc_physics_scene_test.cc` | Schema behavior test.
- `test/plugin/decoder/decoder_test.cc` | Decoder regression checks.

## Behavior-check commands
- `cmake -S . -B build -DMUJOCO_WITH_USD=True && cmake --build build -j`
- `ctest --test-dir build -R "usd|mjcf|decoder" --output-on-failure`
- `python -m pytest python/mujoco/usd/exporter_test.py`
