# Replace Python automation with native Buoy tools

New automation belongs in Buoy or BuoyScript. Each public checkout ignores Python
by default; an individual production exception needs an exact ignore-rule carve
out and a rationale in its `PYTHON.md`. Private reference tests remain local.

The current native paths cover:

- Workspace inventory, verification and assembly; package preparation, catalog
  generation, Python inventory and private issue-queue management.
- Linux BPM and Ahoy builds, including fresh process-host compilation, package
  sealing, target checks and runtime deployment.
- Generic package builds and Linux Studio staging, native bridge compilation,
  dependency sealing and application linking.
- The reusable CI receipt validator, release catalog, website server and website
  content/build helpers.

BPM, Ahoy and buoy-run clients execute natively. Ahoy's recipe engine still uses
its explicitly retained Python helpers. Studio's retained Python producer covers
platform-specific packaging on macOS and Windows; supporting shared modules and
some specialist native builders also remain production exceptions. There are
50 such files across nine source repositories. See the owning `PYTHON.md` files
for the concrete port boundaries; this is not a Python-free distribution yet.

Port remaining behavior in bounded steps, preserving argument arrays, explicit
toolchain overrides, deployment hashes, signing behavior and upstream notices.
Compare actual build and runtime behavior before removing a working fallback.
Cross-target compiler checks supplement native-host execution.

Ahoy is the developer-tool name. The separate buoy-run interpreter remains until
the corresponding script functionality is integrated into the official language
and compiler. Existing recipe aliases and environment overrides remain supported
where they are part of the current interface.

Packages use repository-root manifests; application sources use `src/`.
Workspace assembly preserves logical integration paths for tools that still need
combined sources. It never imports old Git history or publishes private suites.

The highlighting npm package uses `@buoybay/buoy-highlighting` and remains
`private: true` until an npm release is explicitly prepared. GitHub source
publication does not publish npm packages, SDK container images or a live website.
