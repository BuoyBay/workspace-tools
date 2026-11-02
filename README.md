# Manage independent package, IDE and developer-tool checkouts

`workspace-tools` keeps BuoyBay package, IDE and developer-tool repositories
working together. It owns the checkout inventory and native workspace commands;
source stays in each project's repository.

## Set up a workspace

Install Buoy, clone this repository, and build the manager:

```sh
mkdir -p dist/workspace-manager
buoy tools/workspace.bui -o dist/workspace-manager/buoybay
./dist/workspace-manager/buoybay list
```

Clone each listed project using its URL and `repos/<name>` path from
`workspace.json`, then verify and assemble the checkouts:

```sh
./dist/workspace-manager/buoybay verify
./dist/workspace-manager/buoybay assemble
```

`list` shows the selected repositories. `verify` checks source ownership and
required files. `assemble` creates `local/integration/` for build tools that need
combined source inputs. Edit the owning checkout and reassemble between builds.
Keep each executable and its deployed runtime libraries in a separate directory.

[Build hosts and containers](BUILD_HOSTS.md) describes native-worker requirements,
configuration and isolated builds.

## Package and tooling commands

- [PACKAGES.md](PACKAGES.md): package acquisition, metadata and catalog maintenance.
- `tools/package_catalog.bui`: generate package coordinates and audit dependencies.
- `tools/prepare_packages.bui`: prepare dependency metadata and archive-visible licenses.
- [devtools](https://github.com/BuoyBay/devtools): native package and Studio builders.
- [ISSUE_QUEUE.md](ISSUE_QUEUE.md): create and inspect your compiler-issue records.
- [LANGUAGE.md](LANGUAGE.md): editor and GitHub language recognition.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
