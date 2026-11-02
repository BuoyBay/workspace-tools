# Prepare native Linux, macOS and Windows build workers

Configure a worker with the compiler, SDK and native dependencies needed by the
project. The [CI repository](https://github.com/BuoyBay/ci) supplies reusable image
recipes, [example profiles](https://github.com/BuoyBay/ci/blob/main/config/profile.example.json)
and build-receipt validation. Copy an example profile and set executable paths,
runner labels and connection settings for your environment.

## Host requirements

| Native target | Required environment |
| --- | --- |
| Linux | Matching Buoy compiler and SDK, C/C++ toolchain, and package-specific libraries; Rust/Cargo or CMake where a bridge requires them |
| macOS | Matching Buoy compiler and SDK, Xcode command-line tools, and required frameworks/build tools |
| Windows | Matching Buoy compiler and SDK, required C++ runtime, and a supported native C/C++ toolchain |
| Linux containers | A Docker-compatible engine and compiler image for the intended architecture |

GUI applications need a desktop backend and an interactive session or a headless
display setup. Check required screen/input permissions and available services.
Select enough disk space for source snapshots, SDK extraction and output artifacts.
Match the worker and compiler architecture explicitly; identify emulation separately
from native execution.

## Preflight a job

Probe the selected compiler in the same environment that will run the build:

```sh
buoy --version
```

With an explicitly configured Windows compiler:

```powershell
& $env:BUOY_COMPILER --version
```

Record the OS, architecture, compiler and SDK versions, tool versions and free
disk space. Use the build driver's compiler/packager overrides when a wrapper
handles compilation differently from package sealing. Keep that SDK selection
consistent throughout the job.

## Isolate source and outputs

Build the owning checkouts at planned revisions. Compiler packages use root
`manifest.json`, `modules/`, `resources/` and optional `native/`/`runtime/` trees;
application sources use `src/`.

For drivers that need combined sources:

```sh
./dist/workspace-manager/buoybay verify
./dist/workspace-manager/buoybay assemble
```

Assemble between jobs. Give each remote or container job its own source snapshot,
declared dependencies and output directory. Buoy deploys runtime libraries beside
the executable, so unrelated concurrent builds need separate deployment directories.
Collect logs, source/artifact hashes and results before removing that job's caches
and outputs.

## Containers and desktop dependencies

Select images through your profile or the CI recipes. Record the actual image
digest and target architecture. A compiler image that runs Linux binaries serves
a different purpose from an SDK artifact image carrying an installer for another
operating system.

Linux GUI applications may need GTK, WebKit, a compatible JPEG runtime, fonts,
DBus and a display server. Install libraries with the required ABI; renaming an
incompatible shared library does not supply that ABI. Record additional runtime
search paths and dependency versions in the build result.

A Windows console build can run through a configured remote shell. GUI workflows
may need an existing desktop session as well. Configure host trust, VM lifecycle,
desktop access and credentials before starting the build.

## Validate outputs

A build receipt should identify source revision/hashes, native host, compiler and
SDK, exact commands and exits, dependencies, artifact hashes and coverage limits.
Check the application's result marker as well as its process exit status.

Semantic checking, linking, native execution and positive API behavior establish
different properties. Exercise the implementation on its supported platform; an
unsupported-backend result does not validate another platform's implementation.
Match rebuilt native slices to current source and target before packaging them.

Use the [CI receipt gate](https://github.com/BuoyBay/ci) to evaluate the required
build results. Configure artifact publication and deployment as explicit workflow
steps after those checks pass.
