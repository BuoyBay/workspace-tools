# Manage independent Buoy project checkouts

`workspace-tools` keeps the BuoyBay package, IDE and developer-tool repositories
working together. It owns the checkout inventory and native workspace commands;
package and application source stays in each project's own repository.

## Set up a workspace

Clone this repository and build the manager with installed Buoy. `list` shows
publicly listed repositories; clone those entries from `workspace.json` into
their recorded `repos/<name>` paths before verifying or assembling:

```sh
mkdir -p dist/workspace-manager
buoy tools/workspace.bui -o dist/workspace-manager/buoybay
./dist/workspace-manager/buoybay list
./dist/workspace-manager/buoybay verify
./dist/workspace-manager/buoybay assemble
```

`verify` checks source ownership and required files. `assemble` creates the
throwaway `local/integration/` build tree for tools that need combined sources.
Edit the owning checkout, not that generated tree, and reassemble between builds.
Keep each executable and its deployed runtime libraries in a separate directory.
The default commands skip private, held and unlisted repositories. Generated
`buoy-llms.txt` is optional and can be created later with `buoy ai init`.

Authorized maintainers can include private repositories and any available local
test/tooling overlays explicitly:

```sh
./dist/workspace-manager/buoybay list-private
./dist/workspace-manager/buoybay verify-private
./dist/workspace-manager/buoybay assemble-private
```

See [Build hosts and containers](BUILD_HOSTS.md) for native worker requirements
and isolated-build practices. Maintainers keep actual workers, routes and SDK
paths in a private profile and the ignored `BUILD_HOSTS.local.md` operator guide,
provisioned locally during host setup.

## Package and tooling commands

- [PACKAGES.md](PACKAGES.md): package acquisition, metadata and catalog maintenance.
- `tools/package_catalog.bui`: generate package coordinates and audit dependencies.
- `tools/prepare_packages.bui`: prepare package metadata and archive-visible licenses.
- [devtools](https://github.com/BuoyBay/devtools): native package and Studio builders.
- [LANGUAGE.md](LANGUAGE.md): current editor and GitHub language recognition.

The public inventory contains source, docs and required runtime inputs. Internal
tests and detailed evidence stay local; `assemble-private` adds the optional
private test overlay for maintainer validation. It is not needed by public builds.

The native issue-queue tool is public; each maintainer's issue drafts, reviews,
submission records and agent skills stay local and ignored. See
[ISSUE_QUEUE.md](ISSUE_QUEUE.md) to initialize a separate private queue.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

The generated agent files and editor settings stay local and are ignored by Git.

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
