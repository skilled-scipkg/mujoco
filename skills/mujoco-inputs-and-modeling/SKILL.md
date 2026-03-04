---
name: mujoco-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: Inputs and Modeling

## High-Signal Playbook

### Route Conditions
- Use this skill for MJCF/URDF structure, defaults/classes, assets/includes, and compile-time model validation.
- Route installation/build environment issues to `mujoco-build-and-install`.
- Route runtime API coding questions to `mujoco-api-and-scripting`.
- Route schema/changelog/visualization-only narrow topics to `mujoco-advanced-topics`.

### Triage Questions
- Is the model MJCF (`<mujoco>`) or URDF (`<robot>`)? (Ref: `doc/modeling.rst`)
- What is the exact parser/compiler error (row/column and message)? (Ref: `doc/modeling.rst`)
- Are assets/includes resolved from expected paths (`meshdir`, `assetdir`, include tree)? (Refs: `doc/XMLreference.rst`, `doc/modeling.rst`)
- Are you using defaults classes (`class`, `childclass`) intentionally? (Ref: `doc/modeling.rst`)
- Is the issue structural (schema) or physical (instability/divergence)?
- Are you composing repeated structures (`replicate`, `attach`, `include`) that can create naming collisions? (Refs: `doc/XMLreference.rst`, `model/replicate/README.md`)

### Canonical Workflow
1. Start from a minimal compilable MJCF, then grow incrementally. (Refs: `doc/overview.rst`, `doc/modeling.rst`)
2. Validate element/attribute legality in `doc/XMLreference.rst` and schema hierarchy in `doc/XMLschema.rst`.
3. Apply defaults classes deliberately; avoid accidental inheritance bleed-through. (Ref: `doc/modeling.rst`)
4. Add assets and composition (`include`, `replicate`) while keeping names unique and paths explicit. (Refs: `doc/modeling.rst`, `doc/XMLreference.rst`)
5. Compile and run a short simulation; inspect warnings before tuning behavior. (Refs: `doc/programming/simulation.rst`, `doc/modeling.rst`)
6. Cross-check against representative shipped models (`model/humanoid`, `model/replicate`, `model/cube`, `model/adhesion`).
7. If behavior diverges, tune timestep/solver and contact-related options before structural rewrites. (Ref: `doc/modeling.rst`)

### Minimal Working Example
```python
import mujoco

XML = """
<mujoco>
  <worldbody>
    <geom type="plane" size="1 1 0.1"/>
    <body pos="0 0 1">
      <freejoint/>
      <geom type="box" size="0.1 0.2 0.3"/>
    </body>
  </worldbody>
</mujoco>
"""

m = mujoco.MjModel.from_xml_string(XML)
d = mujoco.MjData(m)
for _ in range(200):
  mujoco.mj_step(m, d)
print("final_time", d.time)
```

### Pitfalls and Fixes
- Wrong top-level tag (`<robot>` vs `<mujoco>`): parser infers format from root element, not extension. (Ref: `doc/modeling.rst`)
- Re-including same file or duplicate names after include/replicate: parser/compile errors by design. (Refs: `doc/modeling.rst`, `doc/XMLreference.rst`)
- Expecting attributes to become undefined after setting in defaults: not allowed; reorganize class hierarchy. (Ref: `doc/modeling.rst`)
- Bodies without joints are welded; mobility requires explicit joints/freejoint. (Ref: `doc/modeling.rst`)
- Ignoring compile warnings and proceeding to long runs: run short smoke simulation first and fix warnings early. (Refs: `doc/programming/simulation.rst`, `doc/modeling.rst`)

### Convergence and Validation Checks
- Model compiles cleanly (no parser/compiler error string).
- Short rollout has no repeated instability warnings (`BADQPOS`, `BADQVEL`, `BADQACC`). (Ref: `doc/programming/simulation.rst`)
- Solver iterations remain below configured caps under nominal contacts.
- Save/reload round-trip preserves intended structure/behavior for the edited region.
- For replicated/attached structures, all generated names and references resolve uniquely.

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/modeling.rst`
- `doc/XMLreference.rst`
- `doc/XMLschema.rst`
- `doc/models.rst`
- `doc/programming/simulation.rst`
- `model/humanoid/README.md`
- `model/replicate/README.md`
- `model/cube/README.md`
- `model/adhesion/README.md`

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
- `src/xml/xml_native_reader.cc`
- `src/xml/xml_native_writer.cc`
- `src/xml/xml_urdf.cc`
- `src/xml/xml_api.cc`
- `src/user/user_model.cc`
- `include/mujoco/mjspec.h`
- `model/humanoid/humanoid.xml`
- `model/replicate/scene.xml`
- `model/cube/cube_3x3x3.xml`
- `model/adhesion/active_adhesion.xml`
- `plugin/sdf/register.cc`
- `plugin/sdf/sdf.cc`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
