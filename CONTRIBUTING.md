# Contributing to workspace-tools

The `BuoyBay/workspace-tools` repository manages the checkouts under `repos/`; it does not
own their package or application code. Read the [workspace guide](README.md).
Change code in its owning checkout, and change `workspace.json` only when source
ownership/layout changes. Keep package metadata/catalog coordinates consistent.

## Development conventions

Use the installed Buoy language reference and verify changed `.bui` files with
the compiler. Prefer native Buoy/BuoyScript for new automation. Preserve public
command names, environment-variable contracts and explicit platform limitations.

## Build context

With the compatible installed Buoy toolchain, build the native workspace manager
in its own runtime directory from this checkout:

```sh
mkdir -p dist/workspace-manager
buoy tools/workspace.bui --check --diagnostic-format json
buoy tools/workspace.bui -o dist/workspace-manager/buoybay
./dist/workspace-manager/buoybay verify
```

`verify` checks current source ownership and file presence. It does not establish
that repositories build or pass tests. The manager's `assemble` command writes a
disposable integration tree; edit the owning repositories and do not reassemble
during another build. Prefer native tooling for new package/workspace automation.

## Verification and pull requests

For each edited Buoy entrypoint, in its documented staged dependency context:

```sh
buoy path/to/entrypoint.bui --check --diagnostic-format json
```

Require exit status zero and an empty `errors` array. Read warnings. Check each
affected conditional platform branch with `--target`; compiler checks do not prove
native runtime behavior on another OS. Build/link and exercise the changed API or
workflow on its supported host as well.

Test suites, fixtures and detailed reports are private/ignored for now. Maintainers
run those suites before merge or publication; a fresh checkout is not expected to
contain them. Include your compiler/tool versions, host/target, exact build and
reproduction commands, expected behavior and observed output in the pull request.
Do not commit private tests, generated builds, caches, credentials or machine
configuration. Keep the patch focused and note any unverified platforms or runtime
prerequisites. Inventory checks alone are not build or test acceptance.

## Repository and release workflow

Each owning repository has its own commit/review boundary. Use current source
inventories and package coordinates; keep generated outputs and local agent files
out of commits. Maintainer/agent commits use `Don Lunder <don@zerova.com>` and
GitHub account `donl`; contributors should preserve their own authorship.
Every repository needs actual build and behavioral verification before public
publication. Record platform/runtime requirements and unresolved failures rather
than treating a skipped suite as success.

## License of contributions

Original contributions are accepted under the existing license for the files you
change: `MIT OR Apache-2.0` for first-party code and `CC-BY-4.0` for documentation
prose. Code samples follow their code's license. No CLA or DCO signing step is
required. Keep your own authorship and copyright attribution.

Read [LICENSE](LICENSE) and [LICENSING.md](LICENSING.md). Preserve upstream license,
copyright, attribution and provenance files for third-party material, and identify
its source in the pull request. Submit only material you have the right to license.
Reusable packages and tools keep their permissive licensing boundary; changes to
Studio's AGPL application code belong in its separate repository.
