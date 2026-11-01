# Maintain a private compiler-issue queue

`tools/upstream_issues.bui` validates and renders local issue records. It does
not submit issues or comments to GitHub. Queue contents can include private
reproducers and review evidence, so they stay outside the public source tree.

Compile into an isolated output directory:

```sh
mkdir -p dist/issue-queue
buoy tools/upstream_issues.bui -o dist/issue-queue/upstream-issues
```

For a new queue, create an ignored `local/issues/` directory with an `index.json`
containing the following object. Preserve an existing queue rather than
overwriting its index.

```json
{"schema": 1, "issues": []}
```

```sh
./dist/issue-queue/upstream-issues --queue "$PWD/local/issues" list
./dist/issue-queue/upstream-issues --help
```

Use `--queue` explicitly when working with more than one private queue. The
workspace's default `upstream-issues/` directory is also ignored. Keep private
reproduction files, machine details and submission receipts out of public commits.
