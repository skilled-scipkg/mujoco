---
name: mujoco-build-and-install
description: This skill should be used when users ask about build and install in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: Build and Install

## High-Signal Playbook

### Route Conditions
- Use this skill for MuJoCo installation choice (`pip` vs source), CMake configuration/build/install, and first-run smoke tests.
- Route model authoring questions to `mujoco-inputs-and-modeling`.
- Route Python/MJX API usage questions to `mujoco-api-and-scripting` or `mujoco-mjx`.
- Route USD-specific build and I/O to `mujoco-openusd`.
- Route narrow diagnostics/changelog/xmlschema/visualization deep-dives to `mujoco-advanced-topics`.

### Triage Questions
- What platform and architecture are you on (Linux/macOS/Windows, x86_64/arm64)?
- Do you only need Python bindings (`pip install mujoco`) or full C/C++ source build?
- Do you need optional features (OpenUSD, MJX/Warp, headless rendering)?
- Are you using a release tarball/binary or `main` checkout? (`main` can be unstable; Ref: `README.md`)
- Are failures at configure, compile, runtime launch (`simulate`), or Python import time?
- For Python source build: are `MUJOCO_PATH` and `MUJOCO_PLUGIN_PATH` set? (Ref: `doc/python.rst`)

### Canonical Workflow
1. Start from docs-first install path: release binaries or `pip install mujoco`. (Refs: `README.md`, `doc/python.rst`)
2. For C/C++ source build, configure with CMake and build in `Release`. (Refs: `doc/programming/index.rst`, `CMakeLists.txt`)
3. If needed, install artifacts via `cmake --install` (optionally with `CMAKE_INSTALL_PREFIX`). (Ref: `doc/programming/index.rst`)
4. For Python from source, generate sdist (`python/make_sdist.sh`) and install with `MUJOCO_PATH` and `MUJOCO_PLUGIN_PATH`. (Ref: `doc/python.rst`)
5. Run a native smoke test with `simulate` on a known model. (Refs: `doc/programming/index.rst`, `doc/programming/samples.rst`)
6. Run a Python smoke test (`import mujoco`, load model, one step). (Ref: `doc/python.rst`)
7. If failures persist, cross-check build flags/deps in CMake and only then inspect source entry points.

### Minimal Working Example
```bash
# C/C++ build from source
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j

# Native viewer smoke test (path may vary by generator/platform)
./build/simulate/simulate model/humanoid/humanoid.xml

# Python wheel install path (recommended for bindings-only use)
pip install mujoco
python - <<'PY'
import mujoco
m = mujoco.MjModel.from_xml_path('model/humanoid/humanoid.xml')
d = mujoco.MjData(m)
mujoco.mj_step(m, d)
print('time', d.time)
PY
```

### Pitfalls and Fixes
- Building Python bindings from source when not needed: prefer `pip install mujoco`. (Ref: `doc/python.rst`)
- Missing `MUJOCO_PATH`/`MUJOCO_PLUGIN_PATH` for Python source install: export both before `pip install mujoco-x.y.z.tar.gz`. (Ref: `doc/python.rst`)
- `viewer.launch_passive` on macOS failing from plain `python`: run with `mjpython`. (Ref: `doc/python.rst`)
- Unstable behavior on tip-of-`main`: try a tagged release. (Ref: `README.md`)
- Expecting USD support without enabling it: rebuild with `-DMUJOCO_WITH_USD=True` (and `-Dpxr_DIR=...` when needed). (Ref: `doc/OpenUSD/building.rst`)

### Convergence and Validation Checks
- `simulate` opens and loads a reference model without parser/loader error text.
- `python -c "import mujoco"` works outside the repository tree after install. (Ref: `doc/python.rst`)
- One-step Python smoke test advances `d.time` and emits no fatal warnings.
- Build outputs include expected targets (`mujoco`, `simulate`, samples/tests as configured by CMake options).

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `README.md`
- `doc/programming/index.rst`
- `doc/programming/samples.rst`
- `doc/python.rst`
- `doc/OpenUSD/building.rst`
- `mjx/README.md`
- `doc/mjx.rst`
- `python/README.md`

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
- `CMakeLists.txt`
- `cmake/MujocoDependencies.cmake`
- `cmake/MujocoOptions.cmake`
- `simulate/CMakeLists.txt`
- `simulate/cmake/SimulateDependencies.cmake`
- `sample/CMakeLists.txt`
- `python/setup.py`
- `python/make_sdist.sh`
- `python/mujoco/CMakeLists.txt`
- `python/mujoco/mjpython/mjpython.mm`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" include plugin simulate src python/mujoco wasm/codegen mjx/mujoco/mjx`).
