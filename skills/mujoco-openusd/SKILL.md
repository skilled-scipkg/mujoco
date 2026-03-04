---
name: mujoco-openusd
description: This skill should be used when users ask about openusd in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: Openusd

## High-Signal Playbook

### Route Conditions
- Use this skill for MuJoCo OpenUSD import/export/build questions and `mjcPhysics`-related workflows.
- Route general MuJoCo install/build topics to `mujoco-build-and-install`.
- Route Python API and non-USD scripting questions to `mujoco-api-and-scripting`.
- Route non-USD model semantics to `mujoco-inputs-and-modeling`.

### Triage Questions
- Is this import, export, or build-enablement (`MUJOCO_WITH_USD`) issue?
- Are you using MuJoCo built with USD support, or prebuilt binaries without USD?
- If custom USD build: do you have `pxr_DIR` configured for CMake? (Ref: `doc/OpenUSD/building.rst`)
- Are you importing USD into MJCF (`<asset><model content_type="text/usd" ...`) or drag-dropping into `simulate`? (Ref: `doc/OpenUSD/importing.rst`)
- Are you exporting with Python `USDExporter` and optional dependencies installed (`mujoco[usd]`)? (Ref: `doc/python.rst`)
- Which file types are involved (`.usd`, `.usda`, `.usdc`, `.usdz`)?

### Canonical Workflow
1. Confirm experimental status and version sensitivity first. (Ref: `doc/OpenUSD/index.rst`)
2. Build/enable USD support via CMake (`-DMUJOCO_WITH_USD=True`, optionally `-Dpxr_DIR=...`). (Ref: `doc/OpenUSD/building.rst`)
3. Verify runtime by launching `simulate` and opening/drag-dropping a USD asset. (Ref: `doc/OpenUSD/building.rst`)
4. For MJCF import, reference USD in `<asset><model ... content_type="text/usd"/>`. (Ref: `doc/OpenUSD/importing.rst`)
5. For export, install `mujoco[usd]`, use `mujoco.usd.exporter.USDExporter`, call `update_scene` during stepping, then `save_scene`. (Ref: `doc/python.rst`)
6. If interoperability with USD-native tools is required, consult `mjcPhysics` and MJCF file format plugin docs. (Refs: `doc/OpenUSD/mjcPhysics.rst`, `doc/OpenUSD/mjcf_file_format_plugin.rst`)
7. Escalate to source only when doc behavior is ambiguous.

### Minimal Working Example
```bash
# Enable USD at build time
cmake -S . -B build -DMUJOCO_WITH_USD=True
cmake --build build -j
```

```xml
<mujoco>
  <asset>
    <model file="chair.usdz" name="chair" content_type="text/usd"/>
  </asset>
  <worldbody/>
</mujoco>
```

```python
import mujoco
from mujoco.usd import exporter

m = mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml')
d = mujoco.MjData(m)
exp = exporter.USDExporter(model=m)
for _ in range(120):
  mujoco.mj_step(m, d)
  exp.update_scene(data=d)
exp.save_scene(filetype='usd')
```

### Pitfalls and Fixes
- Assuming OpenUSD is stable API surface: it is explicitly experimental and can change frequently. (Refs: `doc/OpenUSD/index.rst`, `doc/OpenUSD/importing.rst`, `doc/OpenUSD/exporting.rst`)
- Forgetting `MUJOCO_WITH_USD`: USD import path will not be available. (Ref: `doc/OpenUSD/building.rst`)
- Using custom USD build without `pxr_DIR`: CMake cannot locate USD libs. (Ref: `doc/OpenUSD/building.rst`)
- Expecting native non-Python exporter pipeline today: current exporter path is via Python `USDExporter`. (Refs: `doc/OpenUSD/exporting.rst`, `doc/python.rst`)
- Missing optional Python dependencies for export (`usd-core`, `pillow`): install `mujoco[usd]`. (Ref: `doc/python.rst`)

### Convergence and Validation Checks
- CMake configure/build logs show USD-enabled targets are compiled.
- `simulate` accepts USD drag-and-drop/open flow.
- MJCF with `content_type="text/usd"` loads referenced USD asset.
- `USDExporter` emits expected output structure (`assets`, `frames`) and saved USD opens in USD tooling.

## Scope
- Handle questions about documentation grouped under the 'openusd' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/OpenUSD/index.rst`
- `doc/OpenUSD/building.rst`
- `doc/OpenUSD/importing.rst`
- `doc/OpenUSD/exporting.rst`
- `doc/OpenUSD/mjcPhysics.rst`
- `doc/OpenUSD/mjcf_file_format_plugin.rst`
- `doc/python.rst`
- `python/mujoco/usd/README.md`

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
- `src/experimental/usd/CMakeLists.txt`
- `src/experimental/usd/writer.cc`
- `src/experimental/usd/plugins/mjcf/mujoco_to_usd.cc`
- `src/experimental/usd/plugins/mjcf/mjcf_file_format.cc`
- `src/experimental/usd/mjcPhysics/sceneAPI.cpp`
- `plugin/usd_decoder/usd_decoder.cc`
- `plugin/usd_decoder/material_parsing.cc`
- `simulate/main.cc`
- `python/mujoco/usd/exporter.py`
- `python/mujoco/usd/demo.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
