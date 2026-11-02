# Native tooling and compatibility

New automation belongs in Buoy or BuoyScript. Python files are ignored by default;
production exceptions need an exact path and a rationale in the owning
`PYTHON.md`.

The native entrypoints cover workspace inventory and assembly, package preparation
and catalogs, issue-record management, Linux BPM/Ahoy builds, generic package builds,
Linux Studio staging/linking, CI receipts, release catalogs and website helpers.

BPM, Ahoy and buoy-run execute natively. Ahoy's recipe engine still uses its
retained Python helpers. Studio's platform-specific macOS/Windows packaging,
some native-backend builders and several IDE runtime helpers also retain Python
implementations. See [PYTHON.md](PYTHON.md) for the remaining production interfaces.

Port behavior in bounded steps. Preserve argument arrays, toolchain overrides,
deployment hashes, signing behavior and upstream notices. Compare actual build
and runtime behavior before replacing a working entrypoint. Cross-target checks
supplement execution on the supported host.

Ahoy is the developer toolkit; buoy-run runs scripts and the interactive scratchpad.
Existing recipe aliases and environment overrides remain supported where they are
part of the interface. Compiler packages use repository-root manifests and
application sources use `src/`; workspace assembly supplies logical paths for
builders that need combined sources.

The highlighting npm package is `@buoybay/buoy-highlighting`. Its `private: true`
setting prevents accidental npm publication. Source pushes, npm publication,
container-image publication and website deployment are separate operations.
