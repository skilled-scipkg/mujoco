---
name: mujoco-getting-started
description: This skill should be used when users ask about getting started in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: Getting Started

## High-Signal Playbook

### Route Conditions
- Use this skill for first-contact onboarding: choosing entry path (`simulate`, Python, MJX, Unity, OpenUSD).
- Route deep build/install failures to `mujoco-build-and-install`.
- Route model-authoring semantics to `mujoco-inputs-and-modeling`.
- Route API-heavy scripting to `mujoco-api-and-scripting`.
- Route accelerator-specific stepping/perf to `mujoco-mjx`.
- Route USD import/export workflows to `mujoco-openusd`.

### Triage Questions
- Do you want native viewer first (`simulate`) or Python-first workflow? (Refs: `README.md`, `doc/programming/index.rst`)
- Are you using prebuilt release artifacts or building from source?
- Do you already have a model file, or should we start from bundled examples (`model/humanoid`)?
- Are you targeting CPU-only, MJX/JAX, Unity, or OpenUSD?
- Do you need interactive GUI, headless stepping, or batch rollout?
- Are there immediate parser/runtime warnings in the first 100-1000 steps?

### Canonical Workflow
1. Start with a known-good model (`model/humanoid/humanoid.xml`) and run `simulate`. (Refs: `README.md`, `doc/programming/index.rst`)
2. Verify one short Python script can import MuJoCo, load a model, and call `mj_step`. (Ref: `doc/python.rst`)
3. Pick the next workflow branch: modeling, API scripting, MJX, OpenUSD, or Unity. (Refs: `doc/index.rst`, `doc/overview.rst`)
4. Keep docs-first navigation: open topic skill `skills/<skill-id>/references/doc_map.md` before source inspection.
5. Use samples (`sample/basic.cc`, `sample/testspeed.cc`, `sample/compile.cc`) as runnable references. (Ref: `doc/programming/samples.rst`)
6. If instability appears early, reduce scope (short rollout, fewer controls) and inspect warnings first. (Ref: `doc/programming/simulation.rst`)

### Minimal Working Example
```bash
# Native smoke test
./build/simulate/simulate model/humanoid/humanoid.xml

# Python smoke test
python - <<'PY'
import mujoco
m = mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml')
d = mujoco.MjData(m)
for _ in range(10):
  mujoco.mj_step(m, d)
print('ok', d.time)
PY
```

### Pitfalls and Fixes
- Jumping into source before docs: follow docs-first escalation (`doc_map` -> `source_map`).
- Starting from custom complex models too early: begin with bundled model and minimal stepping.
- Passive viewer confusion on macOS: use `mjpython` launcher. (Ref: `doc/python.rst`)
- Assuming `mj_step1`/`mj_step2` works with RK4: use callback path for RK4 controllers. (Ref: `doc/programming/simulation.rst`)
- Treating NumPy views as copies in Python: explicitly `.copy()` when logging history. (Ref: `doc/python.rst`)

### Convergence and Validation Checks
- `simulate` launches and loads bundled model with no load-time errors.
- Python import/load/step succeeds and advances simulation time.
- First short rollout avoids recurring instability warnings.
- Chosen branch skill is identified explicitly (`build`, `modeling`, `api`, `mjx`, `openusd`).

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `README.md`
- `doc/overview.rst`
- `doc/programming/index.rst`
- `doc/programming/simulation.rst`
- `doc/programming/samples.rst`
- `doc/python.rst`
- `doc/modeling.rst`
- `doc/index.rst`

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
- `sample/basic.cc`
- `sample/compile.cc`
- `sample/testspeed.cc`
- `simulate/main.cc`
- `simulate/simulate.cc`
- `python/mujoco/viewer.py`
- `python/mujoco/rollout.cc`
- `include/mujoco/mujoco.h`
- `src/xml/xml_api.cc`
- `mjx/mujoco/mjx/viewer.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
