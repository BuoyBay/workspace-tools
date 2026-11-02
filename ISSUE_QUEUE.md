# Manage compiler-issue records

`tools/upstream_issues.bui` validates and renders issue records. It prepares
submission text and records submission receipts; posting to GitHub is a separate
action.

The tool uses the Crypto package from the matching Buoy SDK. Set
`CRYPTO_ARCHIVE` to that installed `.buipkg` archive, then build in an isolated
directory:

```sh
mkdir -p local/issue-queue-build/packages local/issue-queue-build/bin
cp tools/upstream_issues.bui local/issue-queue-build/main.bui
cp "$CRYPTO_ARCHIVE" local/issue-queue-build/packages/
(
  cd local/issue-queue-build
  buoy main.bui -o bin/upstream-issues
)
```

Choose a directory for your queue. Initialize a new `index.json` with:

```json
{"schema": 1, "issues": []}
```

For a queue created at `local/issues/`:

```sh
./local/issue-queue-build/bin/upstream-issues --queue "$PWD/local/issues" list
./local/issue-queue-build/bin/upstream-issues --help
```

Use `--queue` to select the records to inspect. Preserve an existing index when
adding issues. Keep reproducible inputs, expected and observed behavior, compiler
version and command results with each record. Review generated submission text
before posting and record the returned issue URL afterward.
