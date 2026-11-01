# Prepare native Linux, macOS and Windows build workers

This guide covers reusable host requirements and isolated build practices.
Actual workers, connection routes, installed SDK paths, available disk space and
operator commands belong in a private host profile and the ignored
`BUILD_HOSTS.local.md` operator guide. Provision those locally before scheduling
maintainer jobs; a public clone does not include a working machine inventory.

The public [CI repository](https://github.com/BuoyBay/ci) provides reusable image
recipes, example profiles, receipt validation and a publication gate. It contains
no live host assignments or credentials.

## Host requirements

| Native target | Required environment |
| --- | --- |
| Linux | Matching native Buoy compiler, SDK packages, C/C++ toolchain and any package-specific native dependencies; Rust/Cargo or CMake where a bridge requires them |
| macOS | Matching native Buoy compiler and SDK, Xcode command-line tools, and package-specific build tools and frameworks |
| Windows | Matching native Buoy compiler, SDK, required C++ runtime, and a supported native C/C++ toolchain for bridges |
| Linux containers | A configured Docker-compatible engine and a compiler image for the intended architecture, with its identity recorded |

GUI tests also require a usable desktop backend and an already authorized
interactive session or documented headless setup. A working SSH session does
not establish GUI/input access. Missing permissions or unavailable services are
reported as limits rather than changed automatically by a build.

Select a worker with enough free disk for source snapshots, SDK extraction,
native dependencies and output artifacts. Match the machine and compiler
architecture explicitly. A cross-target compiler check is not native execution;
emulation must be identified in the result.

## Preflight each job

Configure compiler, packager, SDK and connection paths in the private host
profile. Noninteractive sessions may have a different `PATH` from a desktop
shell. Probe the selected executable rather than relying on an old provisioning
status or a previous job's log.

For a compiler available on the current path:

```sh
buoy --version
```

For an explicitly configured Windows compiler:

```powershell
& $env:BUOY_COMPILER --version
```

Record the OS, native architecture, compiler version, SDK identity, required
tool versions and current free disk space. Use the build driver's documented
compiler/packager overrides when a wrapper handles compilation differently from
package sealing. Keep the selected SDK consistent throughout the job.

## Isolate source and outputs

Build the owning checkouts at the planned revisions. Compiler packages use root
`manifest.json`, `modules/`, `resources/` and optional `native/`/`runtime/` trees;
application sources are under `src/`.

The native workspace manager can assemble combined public inputs for drivers
that need them:

```sh
./dist/workspace-manager/buoybay verify
./dist/workspace-manager/buoybay assemble
```

Assemble between jobs, never while another build uses that generated tree.
Remote or container jobs should receive a unique, job-owned source snapshot
containing only the selected project and its declared dependencies. Private
test inputs are supplied separately by an authorized operator; normal public
builds must not depend on them.

Keep each executable and its deployed runtime libraries in a separate output
directory. Buoy deploys libraries beside binaries, so unrelated concurrent
builds must not share that directory.

Use bounded batches when space is limited. Harvest logs, source/artifact hashes,
results and required native slices before deleting that job's extraction caches
and generated binaries. Preserve failure evidence until it has been collected.
Cleanup is limited to the job's own outputs, not shared temporary directories,
other workers' containers or global image caches.

## Containers and desktop backends

Choose images through the configured profile or the CI repository's recipes.
Record the actual image digest and target architecture. A local tag alone is not
an immutable input or evidence that an equivalent public image exists.

Compiler images that execute Linux binaries are distinct from SDK artifact
images carrying installers for another operating system. Artifact transport is
not native compiler execution. Run and label native or emulated checks
accordingly.

Linux GUI validation may need GTK, WebKit, a compatible image runtime, fonts,
DBus and a headless display server. Supply the exact documented dependencies;
an ABI-incompatible library alias does not satisfy a runtime requirement.
Any local runtime overlays or namespace wrappers belong in the private operator
guide and profile, with their source and version recorded in the receipt.

Windows console builds can run through an authorized remote shell, while GUI
tests may need a separate existing desktop session. Host trust, VM lifecycle,
desktop access and credential provisioning are operator responsibilities, not
side effects of reusable build scripts.

## Evidence and publication

Keep detailed tests, snapshots, profiles and logs in ignored local storage. A
build receipt should identify the source revision/hashes, host OS/architecture,
compiler and SDK, exact commands and exits, native dependencies, artifact hashes,
assertions/pass markers, and any coverage limit or skip reason.

Check the application's test result as well as its process exit status.
Semantic checks, linking, native execution and positive API behavior are separate
claims. An unsupported-backend contract does not validate another platform's
implementation. A newly built native bridge must match the current source and
target before its slice is added to a package.

The reusable CI gate checks the required receipts and project holds. It does not
register runners, provision host trust, publish artifacts, change repository
visibility or deploy services automatically. Those are separate operator actions
after validation.
