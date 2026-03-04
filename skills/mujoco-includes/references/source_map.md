# mujoco source map: Includes

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "mujoco-include|rst_prolog|role::|unicode::" doc include/mujoco`
- `rg -n "header_reader|extract|directive" doc/ext`

## Function-level entry points
### Docs include and directive implementation
- `doc/conf.py` | Includes macros/roles via `rst_prolog`.
- `doc/ext/mujoco_include.py` | Custom `mujoco-include` directive logic.
- `doc/ext/header_reader.py` | Header symbol parsing used by docs generation.
- `doc/ext/header_reader_test.py` | Behavior tests for header parsing.

### Source-of-truth include content
- `doc/includes/roles.rst` | Role definitions.
- `doc/includes/macros.rst` | Reusable macro definitions.
- `doc/includes/references.h` | Header references consumed by docs tooling.

### Header/API anchors consumed by docs
- `include/mujoco/mujoco.h` | Primary C API symbols for include extraction.
- `include/mujoco/mjxmacro.h` | Macro definitions referenced by docs.
- `doc/APIreference/functions.rst` | Directive-heavy API reference page.

## Behavior-check commands
- `python -m sphinx -b html doc build/doc-html`
- `python -m pytest doc/ext/header_reader_test.py`
