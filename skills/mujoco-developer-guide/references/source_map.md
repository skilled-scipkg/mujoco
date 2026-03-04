# mujoco source map: Developer Guide

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" include/mujoco plugin src/engine src/experimental/usd test/plugin test/experimental/usd`
- `rg -n "mjp|plugin|register|resource|decoder|SdfFileFormat" include/mujoco plugin src/engine src/experimental/usd`

## Function-level entry points
### Extension/public contracts
- `include/mujoco/mjplugin.h` | Public plugin ABI and callback contract.
- `src/engine/engine_plugin.h` | Engine-side plugin interfaces.
- `src/engine/engine_plugin.cc` | Plugin registration/dispatch implementation.

### Example plugin implementations
- `plugin/sensor/register.cc` | Sensor plugin registration.
- `plugin/sensor/touch_grid.cc` | Sensor plugin behavior.
- `plugin/actuator/register.cc` | Actuator plugin registration.
- `plugin/actuator/pid.cc` | PID actuator plugin logic.
- `plugin/elasticity/register.cc` | Elasticity plugin registration.
- `plugin/elasticity/elasticity.cc` | Elasticity plugin implementation.

### USD extension internals
- `src/experimental/usd/plugins/mjcf/mjcf_file_format.cc` | MJCF file-format plugin entry.
- `src/experimental/usd/plugins/mjcf/mujoco_to_usd.cc` | MJCF -> USD conversion.
- `src/experimental/usd/plugins/mjcf/utils.cc` | Shared plugin utilities.

### Behavior/regression checks
- `test/plugin/actuator/pid_test.cc`
- `test/plugin/sensor/sensor_test.cc`
- `test/plugin/elasticity/elasticity_test.cc`
- `test/experimental/usd/plugins/mjcf/mjcf_file_format_test.cc`

## Behavior-check commands
- `ctest --test-dir build -R "plugin|usd|mjcf" --output-on-failure`
- `./build/simulate/simulate model/humanoid/humanoid.xml`
