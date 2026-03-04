---
name: mujoco-advanced-topics
description: Consolidated skill for narrow MuJoCo topics that were previously single-doc skills (troubleshooting, analysis/output, xmlschema, changelog, examples/tutorials, and general routing docs).
---

# mujoco: Advanced Topics

## Scope
- Handle narrow or low-frequency topics that previously mapped to single-doc skills.
- Keep answers docs-first and route back to core skills when the request expands beyond the narrow topic.

## Route Conditions
- Use this skill for: changelog lookups, raw XML schema table navigation, visualization/output internals, docs-index routing/meta docs, and static saved example references.
- Route installation/build/setup to `mujoco-build-and-install`.
- Route modeling semantics to `mujoco-inputs-and-modeling`.
- Route API coding to `mujoco-api-and-scripting`.
- Route accelerator paths to `mujoco-mjx`.
- Route OpenUSD workflows to `mujoco-openusd`.

## High-Signal Playbook
- Identify which narrow bucket applies; do not mix multiple broad topics in this skill.
- Reproduce quickly on a known model before deep inspection:
  `./build/simulate/simulate model/humanoid/humanoid.xml`.
- For XML/schema or save-output checks, compile and save once:
  `./build/sample/compile model/humanoid/humanoid.xml build/humanoid_compiled.xml`.
- If the issue requires broader install/modeling/API reasoning, route to the matching core skill immediately.

## Validation Checkpoints
- For visualization topics: viewer loads and steps without runtime errors.
- For schema/serialization topics: compile/save command emits output file and no parser errors.
- For changelog/routing topics: answer cites exact docs path and relevant version section.

## Topic Buckets
- Troubleshooting environment/doc deps: `doc/requirements.txt`
- Analysis and output/visualization internals: `doc/programming/visualization.rst`
- Raw MJCF schema tree: `doc/XMLschema.rst`
- Release notes and migration context: `doc/changelog.rst`
- Static canonical saved example artifact: `doc/_static/example_saved.txt`
- Docs toctree and top-level routing: `doc/index.rst`

## Primary documentation references
- `doc/requirements.txt`
- `doc/programming/visualization.rst`
- `doc/XMLschema.rst`
- `doc/changelog.rst`
- `doc/_static/example_saved.txt`
- `doc/index.rst`

## Workflow
- Start with the specific doc file matching the bucket above.
- If the request broadens, route immediately to the matching core skill instead of overextending this skill.
- If documentation is insufficient, inspect `references/source_map.md` for concrete source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `sample`
- `model`
- `python`
- `mjx`

## Source entry points for unresolved issues
- `simulate/simulate.cc`
- `simulate/main.cc`
- `src/engine/engine_forward.c`
- `src/xml/xml_native_reader.cc`
- `src/xml/xml_native_writer.cc`
- `sample/compile.cc`
- `sample/basic.cc`
- `python/mujoco/viewer.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
