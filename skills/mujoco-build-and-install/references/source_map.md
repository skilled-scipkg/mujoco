# mujoco source map: Build and Install

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMakeLists.txt cmake simulate sample python/mujoco src/experimental/usd plugin`
- `rg -n "MUJOCO_WITH_USD|CMAKE_INSTALL_PREFIX|add_subdirectory|target_link_libraries" CMakeLists.txt cmake simulate sample python/mujoco`

## Function-level entry points
### Top-level build and options
- `CMakeLists.txt` | Root target graph and build defaults.
- `cmake/MujocoDependencies.cmake` | Third-party dependency resolution.
- `cmake/MujocoOptions.cmake` | Feature flags and build options.
- `simulate/CMakeLists.txt` | Native viewer target wiring.
- `simulate/cmake/SimulateDependencies.cmake` | `simulate` dependency management.
- `sample/CMakeLists.txt` | Sample binaries target wiring.

### Optional USD and plugin build paths
- `src/experimental/usd/CMakeLists.txt` | USD feature target integration.
- `plugin/usd_decoder/CMakeLists.txt` | USD decoder plugin target.

### Python package build/install
- `python/setup.py` | Python package build entrypoint.
- `python/mujoco/CMakeLists.txt` | Python extension target definition.
- `python/make_sdist.sh` | Source-distribution packaging path.
- `python/mujoco/mjpython/mjpython.mm` | macOS `mjpython` launcher behavior.

### Runtime smoke-path entry points
- `simulate/main.cc` | Launch and command-line handling.
- `simulate/simulate.cc` | Model load and simulation loop.
- `sample/basic.cc` | Minimal native step path.

## Behavior-check commands
- `cmake -S . -B build -DCMAKE_BUILD_TYPE=Release`
- `cmake --build build -j`
- `./build/simulate/simulate model/humanoid/humanoid.xml`
- `python -c "import mujoco; print(mujoco.__version__)"`
