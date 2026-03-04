# mujoco source map: Inputs and Modeling

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" src/xml src/user include/mujoco plugin/sdf model test/xml test/user`
- `rg -n "parse|compile|include|defaults|class|replicate|URDF|mesh|hfield" src/xml src/user`

## Function-level entry points
### Parser/compiler path for MJCF/URDF
- `src/xml/xml_native_reader.cc` | Core XML read and compile pipeline.
- `src/xml/xml_native_writer.cc` | XML save/round-trip serialization.
- `src/xml/xml_urdf.cc` | URDF conversion pipeline.
- `src/xml/xml_api.cc` | Public parser/compile wrapper APIs.
- `src/xml/xml_util.cc` | XML helper routines for validation/path handling.

### Model construction and user-layer semantics
- `src/user/user_model.cc` | Model object assembly and validation.
- `src/user/user_objects.cc` | Element/object handling and defaults propagation.
- `src/user/user_composite.cc` | Composite model generation behavior.
- `src/user/user_flexcomp.cc` | Flex/composite authoring internals.
- `src/user/user_api.cc` | Programmatic model editing entry points.
- `include/mujoco/mjspec.h` | Spec API declarations tied to authoring.

### Plugin/model examples tied to modeling behavior
- `plugin/sdf/register.cc` | SDF plugin registration.
- `plugin/sdf/sdf.cc` | SDF behavior implementation.
- `plugin/sdf/gear.cc` | Concrete SDF geometry behavior.
- `model/humanoid/humanoid.xml` | Canonical complex model example.
- `model/replicate/scene.xml` | Replicate/include composition example.
- `model/cube/cube_3x3x3.xml` | Structured repeated geometry example.

### Behavior/regression checks
- `test/xml/xml_native_reader_test.cc`
- `test/xml/xml_urdf_test.cc`
- `test/xml/xml_api_test.cc`
- `test/user/user_model_test.cc`
- `test/user/user_api_test.cc`

## Behavior-check commands
- `./build/sample/compile model/humanoid/humanoid.xml build/humanoid_compiled.xml`
- `ctest --test-dir build -R "xml_|user_" --output-on-failure`
