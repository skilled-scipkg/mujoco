# mujoco source map: MJX

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" mjx/mujoco/mjx/_src mjx/mujoco/mjx/warp mjx/mujoco/mjx/integration_test`
- `rg -n "put_model|put_data|make_data|step|solver|collision|graph_mode" mjx/mujoco/mjx`

## Function-level entry points
### JAX-backed MJX core
- `mjx/mujoco/mjx/_src/io.py` | Model/data conversion between MuJoCo and MJX.
- `mjx/mujoco/mjx/_src/forward.py` | Forward stepping pipeline.
- `mjx/mujoco/mjx/_src/solver.py` | Constraint solver behavior.
- `mjx/mujoco/mjx/_src/collision_driver.py` | Collision dispatch pipeline.
- `mjx/mujoco/mjx/_src/constraint.py` | Constraint assembly behavior.

### Warp-backed MJX implementation
- `mjx/mujoco/mjx/warp/io.py` | Warp-side model/data IO.
- `mjx/mujoco/mjx/warp/forward.py` | Warp stepping path.
- `mjx/mujoco/mjx/warp/ffi.py` | Warp FFI and graph execution mode.
- `mjx/mujoco/mjx/warp/collision_driver.py` | Warp collision driver.
- `mjx/mujoco/mjx/warp/render_context.py` | Render context/world sizing behavior.

### Tooling and behavior checks
- `mjx/mujoco/mjx/viewer.py`
- `mjx/mujoco/mjx/testspeed.py`
- `mjx/mujoco/mjx/_src/io_test.py`
- `mjx/mujoco/mjx/_src/forward_test.py`
- `mjx/mujoco/mjx/_src/solver_test.py`
- `mjx/mujoco/mjx/integration_test/forward_test.py`

## Behavior-check commands
- `python -m pytest mjx/mujoco/mjx/_src/forward_test.py -k step`
- `python -m pytest mjx/mujoco/mjx/_src/solver_test.py`
- `python -m pytest mjx/mujoco/mjx/warp/forward_test.py`
