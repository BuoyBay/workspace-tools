# Installing BuoyBay packages

Buoy package checkouts use individual repositories. The compiler
`manifest.json`, `modules/`, `resources/`, and any native inputs live at the
repository root. BPM installs directly from that root; no `subdir` is needed.
The source `manifest.json` version identifies package contents. It does
not establish a tested release by itself.

`packages.json` records exact acquisition coordinates from local repository HEADs,
manifest versions, expected version tag names, and a limited metadata audit. Its
`source` objects can be copied directly into a consumer's `bpm.json` dependencies.
Check that the recorded revisions have been pushed before installing them remotely.
The catalog follows the listing and release-selection settings in `workspace.json`.
It is an acquisition inventory; consult each package's requirements and release
notes before selecting a version.

## Install from an exact commit

Build BPM as described in its [build guide](https://github.com/BuoyBay/bpm/blob/main/src/README.md).
From an empty consumer directory:

```sh
bpm init
bpm add mathkit --git https://github.com/BuoyBay/mathkit.git \
  --rev FULL_COMMIT_FROM_PACKAGES_JSON
bpm verify
```

Replace the revision placeholder with `mathkit`'s full `source.rev` value from the
catalog. Commit the resulting `bpm.json` and `bpm.lock` together. `bpm resolve`
records exact Git revisions, source hashes and archive hashes. `bpm install`
restores a locked selection; `bpm vendor` retains exact package archives for
offline `bpm ci`.

Set `BPM_PACKAGER` to the installed executable that supports `pkg seal` when
your compile wrapper cannot dispatch package commands. Keep your compiler
selection for ordinary builds.

Pure packages such as MathKit or AlgorithmKit are useful first consumer checks.
Install one into a fresh directory and build a consumer using its documented API.
A successful source installation does not establish native platform support for
packages with FFI or bundled runtimes.

## Dependency metadata and release tags

A dependency value must be a source object, for example:

```json
{
  "schema": 1,
  "dependencies": {
    "mathkit": {
      "git": "https://github.com/BuoyBay/mathkit.git",
      "rev": "refs/heads/main"
    }
  },
  "allowHosts": ["github.com"]
}
```

The initial package manifests use `refs/heads/main` for sibling development
dependencies. Explicit resolution follows that branch and locks the resulting
commit. Review lock updates before accepting changed transitive code. A string
such as `"mathkit": "0.1.0"` is not a supported BPM source declaration.

Once a package has passed release validation, BPM's version-based source form
uses `"version": "0.1.0"` instead of `rev`. BPM expects the repository-root tag
`v0.1.0`. The manifest version must agree
with the requested tag version. Do not move published version tags. No release
tags are created by package preparation.

## Native inputs and compiler packages

Package `files` lists retain compiler source, resources, native runtime/link
inputs, package-local licenses, and upstream provenance notices. Dependency
acquisition does not build native bridges or run dependency scripts. Read each
package's platform prerequisites before building a consumer.

License texts and notices are also retained under `resources/buoybay-licenses/`.
The compiler's archive sealer preserves known package directories and drops
arbitrary root files, so root license files alone are insufficient for `.buipkg`
distribution. Keep the resource copies in sync when terms or notices change.

Compiler-supplied packages such as `crypto` are separate from BuoyBay packages.
They must come from the installed Buoy toolchain; no fabricated BuoyBay Git
origins are provided for them.

HTTPKit remains blocked for ordinary runtime use until its documented modified
`HTTPServer-1.1.0-httpkit.1.buipkg` is explicitly built and installed. The standard
installed HTTPServer archive is insufficient. Read its package-local
`TOOLCHAIN-REQUIREMENTS.md` and compatibility instructions before using it.

## Maintaining package metadata

`tools/prepare_packages.bui` prepares package-local dual licenses, documentation
licensing, source dependency objects and import-derived sibling dependencies.
Its default selection follows `workspace.json`. Use `--include-private` when
explicitly preparing additional configured entries. Preserve each project's
license policy when selecting inputs.
It updates current physical file ownership and logical integration paths in
`workspace.json`. It does not change compiler
manifest versions, create tags, stage files, commit, or push. Review dependency decisions before running it
again: its initial preparation policy selects sibling `main` refs.

Compile the preparation helper directly with the installed Buoy compiler:

```sh
mkdir -p dist/package-preparation
buoy tools/prepare_packages.bui --check --diagnostic-format json
buoy tools/prepare_packages.bui -o dist/package-preparation/prepare-packages
./dist/package-preparation/prepare-packages "$PWD"
```

After reviewing, committing and pushing package changes, refresh immutable
catalog coordinates with the native catalog helper:

```sh
buoy tools/package_catalog.bui --check --diagnostic-format json
mkdir -p dist/package-catalog-cli local
buoy tools/package_catalog.bui -o dist/package-catalog-cli/package-catalog
./dist/package-catalog-cli/package-catalog audit "$PWD"
./dist/package-catalog-cli/package-catalog "$PWD" > local/packages.json.staging && \
  mv local/packages.json.staging packages.json
```

The import scan finds direct sibling imports in package modules, including
conditional branches. It cannot prove complete semantic dependency closure,
compiler-supplied package availability, native ABI correctness, runtime behavior,
or completeness of third-party attribution. Retain meaningful package checks
alongside metadata validation. Every repository must pass actual builds and tests
before publication; catalog generation or a successful metadata audit does not
authorize publishing a repository.
