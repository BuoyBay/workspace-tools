# Buoy language metadata and native tooling

The source language and the GitHub language badge are separate concerns. Convert
operational Python entrypoints into working Buoy or BuoyScript executables, then
retire Python implementations after their behavior is covered by the native path.
Keep existing Python regression tests while they still verify useful contracts.

As checked on 2026-11-01, GitHub Linguist's current language registry has no entry
for Buoy, BuoyScript, or `.bui`. Reducing Python therefore cannot by itself make
GitHub's language bar report Buoy. The registry is the authoritative list of
recognized languages. [Linguist language registry](https://github.com/github-linguist/linguist/blob/main/lib/linguist/languages.yml)

## Repository discovery now

Use accurate repository descriptions and topics while the language is new.
GitHub supports topics for a project's language, purpose, and community; topics
are searchable and appear on repository pages. Topics do not set the language
statistics. [GitHub topic documentation](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics)

Recommended common topics are `buoy` and `buoy-language`. Add
`buoy-package` to compiler packages. Use `buoyscript` where the project actually
supports scripts or script execution, rather than adding it to every native
library. Add purpose-specific topics such as `package-manager`, `developer-tools`,
`ide`, `syntax-highlighting`, or `gtk` when appropriate. Leave the organization
name off the topic list.

Descriptions should identify the package's concrete purpose and the language, for
example: "Generic ordered search and stable sorted copies for Buoy". Studio can
say "Buoy Bay Studio: native desktop IDE for Buoy and BuoyScript". A C helper can
say "GTK lifecycle integration used by Buoy desktop tooling" without claiming its
implementation is Buoy. A description and topic update can be performed with the
existing authenticated GitHub CLI; it does not require a new automation runtime.

## Accurate source classification

Keep `.bui` as the real source extension. Do not map it to Visual Basic, Python,
or another recognized language merely to obtain highlighting or a badge. An
unrecognized `linguist-language=Buoy` override also cannot add Buoy to statistics.
Linguist documents that unknown languages remain excluded even when marked
detectable. [Linguist overrides](https://github.com/github-linguist/linguist/blob/main/docs/overrides.md#detectable)

Use `.gitattributes` overrides only for their actual meaning: upstream source may
be marked `linguist-vendored`, and genuinely generated output may be marked
`linguist-generated`. First-party Python builders and tests remain first-party
source until ported. Bundled native C and Rust helpers remain their actual
languages. [Linguist classification overrides](https://github.com/github-linguist/linguist/blob/main/docs/overrides.md)

One verified vendor candidate is Scene3DKit's
`repos/scene3dkit/native/vendor/stb_image.h`, whose adjacent `PROVENANCE.json`
records upstream commit, hash, and license. A repository-local attribute could
mark that exact vendor tree. Preserve its code and notices. Do not mark all
`native/` code as vendored: it also contains original bridges. No attributes were
added during this audit.

## Future Linguist support

The workspace already contains a TextMate grammar at
`repos/buoy-highlighting/buoy.tmLanguage.json`, with
scope `source.buoy`. That is a useful starting input for a future Linguist
contribution; its compatibility and license still need to satisfy the upstream
submission process.

Linguist currently requires a language definition, accepted highlighting grammar,
representative licensed samples, generated identifier, tests, and evidence of
public usage. Its current usage policy asks for at least 2,000 indexed files in
the last year for extensions normally repeated within a repository and considers
distribution across independent users and repositories. Creating many repos in
one organization does not establish that distribution. Prepare upstream support
when real usage meets the policy; publishing these packages does not guarantee
acceptance or immediate GitHub deployment. [Linguist contribution policy](https://github.com/github-linguist/linguist/blob/main/CONTRIBUTING.md#adding-a-language)

## Local audit scope

The current public Python inventory contains 50 explicitly retained production
files across nine repositories, documented in [PYTHON.md](PYTHON.md). Dedicated
tests and validation generators remain private and ignored. Native ports change
the implementation language; metadata alone cannot add a language to Linguist.
This workspace does not run or modify GitHub's deployed language detector.
