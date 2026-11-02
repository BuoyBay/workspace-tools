# Public Python policy

Python files are ignored by default in this workspace and all project repositories.
New tooling should use native Buoy or BuoyScript. A public Python file needs an exact-path
ignore-rule exception and a concrete production build, runtime, or unported-platform reason.

Native entrypoints are the normal Linux paths. The exact exceptions below
preserve required production capabilities while their native implementations mature.

Root workspace tooling has no public Python exceptions. Each project records its exceptions
in its own `PYTHON.md`; `.gitignore` contains only the matching exact paths.

## Current production exceptions

| Repository | File | Required capability |
| --- | --- | --- |
| `ahoy` | `tools/buoy_build.py` | Compatibility entrypoint for Ahoy recipe planning, execution records and cleanup; deployed by the native Ahoy builder. |
| `ahoy` | `tools/buoydev/__init__.py` | Package initializer required by the deployed Ahoy recipe helper imports. |
| `ahoy` | `tools/buoydev/build.py` | Existing recipe planner, dependency ordering, locking and build receipts; this feature still needs its native engine port. |
| `ahoy` | `tools/buoydev/common.py` | Atomic files and bounded subprocess/configuration helpers imported by the existing Ahoy recipe engine. |
| `bpm` | `src/registry/build.py` | Only current builder for the optional Linux reference registry server and its native storage bridge. |
| `bpm` | `tools/build_bpm.py` | Unported macOS, Windows and cross-target BPM bootstrap, including platform runtime deployment; Linux uses build_bpm.bui. |
| `bpm` | `tools/build_bpm_host.py` | Native bridge compilation and packaging dependency of the unported macOS/Windows BPM bootstrap. |
| `devtools` | `tools/build_audiocuekit.py` | Builds and seals the AudioCueKit native audio bridge for selected supported platforms. |
| `devtools` | `tools/build_chatgptauth.py` | Builds and seals the ChatGPTAuth native cryptographic verification bridge, including release target receipts. |
| `devtools` | `tools/build_classic_processkit_slices.py` | ProcessKit compatibility slice adapter imported by the retained RBClassic package staging build. |
| `devtools` | `tools/build_compressionkit.py` | Rebuilds CompressionKit native codec bridges and packages verified target slices. |
| `devtools` | `tools/build_compressionkit_arm.py` | Cross/native ARM CompressionKit slice provisioning not implemented by the generic Buoy builder. |
| `devtools` | `tools/build_font_arm.py` | ARM FontKit/ShapingKit native slice collection not covered by the generic source package builder. |
| `devtools` | `tools/build_fontcanvas.py` | Builds the Windows FontCanvas C++/GDI+ bridge and validates its native architecture. |
| `devtools` | `tools/build_fontkit.py` | Rebuilds platform font/text native bridges and verifies package runtime artifacts. |
| `devtools` | `tools/build_gitkit.py` | Rebuilds GitKit native Git bridge slices and verifies their target receipts. |
| `devtools` | `tools/build_keychain.py` | Builds the platform credential-store native bridges; generic source sealing cannot rebuild these. |
| `devtools` | `tools/build_odbckit.py` | Builds the ODBC native bridge and validates its source/binary receipt before sealing. |
| `devtools` | `tools/build_previewkit.py` | PreviewKit source-stage adapter imported by the retained RBClassic build; pure source package builds use the native driver. |
| `devtools` | `tools/build_processkit.py` | Rebuilds ProcessKit native process bridge slices for package/release targets not covered by Linux tool bootstrap. |
| `devtools` | `tools/build_rbclassic.py` | Stages the REALbasic compatibility package with platform adapters and verified ProcessKit/lifecycle dependencies. |
| `devtools` | `tools/build_renderprocess.py` | Compiles the native preview render-process bridge for the selected target. |
| `devtools` | `tools/build_scene3dimages.py` | Compiles Scene3DKit native image-codec bridge slices. |
| `devtools` | `tools/build_scene3dkit.py` | Coordinates fresh GPU/image bridges and Scene3DKit target-specific package sealing. |
| `devtools` | `tools/build_scene3dkit_native.py` | Compiles Scene3DKit native GPU bridge slices for supported targets. |
| `devtools` | `tools/build_shapingkit.py` | Builds the platform text-shaping native bridge and verifies package artifact receipts. |
| `devtools` | `tools/build_studio.py` | Unported macOS/Windows Studio build, SDK and release modes; normal Linux builds use build_studio.bui. |
| `devtools` | `tools/build_studio_script_runner.py` | Compiles Studio optional native BuoyScript SDK runner; this SDK path is not in the new Linux builder. |
| `devtools` | `tools/build_support.py` | Shared verified artifact/sealing/architecture helpers required by retained native bridge builders and Ahoy recipe compatibility. |
| `devtools` | `tools/build_terminalkit.py` | Builds TerminalKit native terminal/PTY bridge target slices. |
| `devtools` | `tools/build_textmetrics.py` | Builds and packages native platform text-metrics bridge slices, including macOS. |
| `devtools` | `tools/build_thinkgearkit.py` | Coordinates native ThinkGear bridge source builds and package runtime publication. |
| `devtools` | `tools/build_treesitter.py` | Builds pinned tree-sitter native parser/runtime slices for package consumers. |
| `devtools` | `tools/build_updatekit.py` | Builds the UpdateKit Rust/native update bridge and retains dependency license receipts. |
| `devtools` | `tools/buoy_executables.py` | Shared compiler/packager resolution and toolchain argument handling required by retained builders and the Ahoy recipe engine. |
| `devtools` | `tools/development_permissions.py` | Production/development credential profile staging required by the retained Studio platform build fallback. |
| `devtools` | `tools/discover_documentation.py` | Bundled Studio runtime helper for package documentation discovery and cache generation. |
| `devtools` | `tools/discover_layout_packages.py` | Bundled Studio runtime helper for safe layout/plugin metadata discovery; called by current Studio source. |
| `devtools` | `tools/font_package_artifacts.py` | Shared font/shaping/text native slice receipts and architecture verification imported by retained native builders. |
| `devtools` | `tools/studio_build_info.py` | Build identity/resource generation required by the retained Studio platform build fallback. |
| `devtools` | `tools/studio_debug_adapter.py` | Bundled Studio runtime DAP bridge; current debugger starts this helper explicitly. |
| `devtools` | `tools/studio_release_packages.py` | Per-target SDK staging implementation used by retained Studio release builds; invokes explicit native package recipes. |
| `devtools` | `tools/updatekit_licenses.py` | Imported production dependency of build_updatekit.py; retained until that entrypoint and its dependency are ported together. |
| `devtools` | `tools/updatekit_release.py` | Current release artifact publication/signing orchestration for UpdateKit; not covered by native source build tooling. |
| `gtklifecycle` | `tools/build_gtklifecycle.py` | Current standalone source compiler for the shared GTK lifecycle guard object used by Linux desktop packages. |
| `httpkit` | `compat/build.py` | Builds the pinned HTTPServer compatibility archive required by HTTPKit; checks the upstream source digest before applying its framing patch. |
| `native-integrations` | `containers/integrations/pdf-patch/build-pdf-package.py` | Builds the explicit native PDF compatibility archive needed by the PDF integration; no native replacement exists yet. |
| `notifications` | `build_native.py` | Only current source builder for the macOS UserNotifications Objective-C bridge and its receipt. |
| `poweractivity` | `build_native.py` | Only current native source builder for PowerActivity platform bridge slices. |
| `thinkgearkit` | `build_native.py` | Only current compiler/receipt implementation for ThinkGear transport and discovery native bridges. |

## Review rule

For each proposed Python file, identify its public caller and supported capability, verify its
local import and subprocess-source dependencies, and specify which native implementation
will replace it.
Remove the exception after parity checks pass. Do not add `!*.py` or directory-wide allow rules.
