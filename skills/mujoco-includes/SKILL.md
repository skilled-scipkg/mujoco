---
name: mujoco-includes
description: This skill should be used when users ask about includes in mujoco; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mujoco: Includes

## High-Signal Playbook

### Route Conditions
- Use this skill for documentation include/role/macro issues (`doc/includes`) and `mujoco-include` directive behavior.
- Route API semantics questions to `mujoco-api-and-scripting`.
- Route runtime simulation behavior to `mujoco-getting-started` or `mujoco-inputs-and-modeling`.

### Triage Questions
- Is the issue an unknown role/substitution in docs rendering?
- Is the issue in `.. mujoco-include::` API extraction from headers?
- Is failure during docs build only, or also in runtime simulation code?

### Canonical Workflow
1. Confirm the requested role/substitution exists in `doc/includes/roles.rst` or `doc/includes/macros.rst`.
2. Verify `doc/conf.py` includes both files in `rst_prolog`.
3. For `mujoco-include` issues, inspect `doc/ext/mujoco_include.py` and `doc/ext/header_reader.py`.
4. Cross-check referenced API symbols in `include/mujoco/mujoco.h` and related headers.
5. Validate by running a docs build and resolving warnings before touching unrelated runtime code.

### Minimal Working Checks
```bash
python -m sphinx -b html doc build/doc-html
./build/simulate/simulate model/humanoid/humanoid.xml
```

### Convergence and Validation Checks
- Docs build completes without unknown role/substitution/include directive errors.
- Any `mujoco-include` symbol resolves from the expected header location.
- Runtime smoke simulation still runs after doc-tooling updates.

## Scope
- Handle questions about documentation grouped under the 'includes' theme.
- Keep responses concise; this is a docs tooling skill, not a runtime simulation API skill.

## Primary documentation references
- `doc/includes/roles.rst`
- `doc/includes/macros.rst`
- `doc/conf.py`
- `doc/ext/mujoco_include.py`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Optional deeper inspection
- `doc/ext/header_reader.py`
- `include/mujoco`
- `test`

## Source entry points for unresolved issues
- `doc/conf.py`
- `doc/ext/mujoco_include.py`
- `doc/ext/header_reader.py`
- `doc/ext/header_reader_test.py`
- `doc/includes/roles.rst`
- `doc/includes/macros.rst`
- `include/mujoco/mujoco.h`
- `include/mujoco/mjxmacro.h`
- `doc/APIreference/functions.rst`
- Prefer targeted source search (for example: `rg -n "mujoco-include|rst_prolog|role::|unicode::" doc include/mujoco`).
