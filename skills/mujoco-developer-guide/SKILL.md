---
name: mujoco-developer-guide
description: This skill should be used when users ask about developer guide in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: Developer Guide

## High-Signal Playbook

### Route Conditions
- Use this skill for engine/plugin architecture, extension points, and contributor-facing internals.
- Route end-user build/install failures to `mujoco-build-and-install`.
- Route user-level API usage to `mujoco-api-and-scripting`.
- Route OpenUSD workflow usage to `mujoco-openusd` (keep this skill for internals/implementation details).

### Triage Questions
- Is the request about engine plugins, custom resources/decoders, or USD file-format plugin internals?
- Are you modifying extension APIs (`mjpPlugin`, decoder/resource hooks) or only using existing plugins?
- Do you need behavior validation via existing plugin/USD tests before coding changes?

### Canonical Workflow
1. Start with extension architecture docs and plugin contracts. (Refs: `doc/programming/extension.rst`, `doc/OpenUSD/mjcf_file_format_plugin.rst`)
2. Trace the public interface first (`include/mujoco/mjplugin.h`), then engine/plugin bridge implementation.
3. Reproduce behavior with the smallest existing plugin or USD test.
4. Apply changes near the owning extension layer, not at unrelated call sites.
5. Re-run focused tests and confirm `simulate` still loads a reference model.

### Minimal Working Checks
```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j
ctest --test-dir build -R "plugin|usd|mjcf" --output-on-failure
./build/simulate/simulate model/humanoid/humanoid.xml
```

### Convergence and Validation Checks
- Plugin registration path is unchanged or intentionally updated and covered by tests.
- Focused plugin/USD tests pass without introducing unrelated failures.
- `simulate` still loads and steps a bundled model after extension-layer edits.

## Scope
- Handle questions about developer architecture, extension points, and contribution workflow.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/programming/extension.rst`
- `doc/OpenUSD/mjcf_file_format_plugin.rst`
- `CONTRIBUTING.md`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tests as behavior/regression references before patching internals.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `sample`
- `simulate`
- `plugin`

## Test references
- `test/plugin`
- `test/experimental/usd`

## Optional deeper inspection
- `include`
- `plugin`
- `src`
- `simulate`
- `python/mujoco`

## Source entry points for unresolved issues
- `include/mujoco/mjplugin.h`
- `src/engine/engine_plugin.h`
- `src/engine/engine_plugin.cc`
- `plugin/sensor/register.cc`
- `plugin/actuator/register.cc`
- `plugin/elasticity/register.cc`
- `src/experimental/usd/plugins/mjcf/mjcf_file_format.cc`
- `src/experimental/usd/plugins/mjcf/mujoco_to_usd.cc`
- `src/experimental/usd/plugins/mjcf/utils.cc`
- `test/experimental/usd/plugins/mjcf/mjcf_file_format_test.cc`
- `test/plugin/decoder/decoder_test.cc`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
