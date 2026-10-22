# Repository Hygiene

Engineering policy for what belongs in source control: inclusion and exclusion,
generated files, local environments, `.gitignore`, large and scientific data, staging,
untracking, structure and commit-content review. Secrets and accidental publication of
sensitive content are handled under [SECURITY](SECURITY.md); commit authorization and
messages under [GIT_OPERATIONS](GIT_OPERATIONS.md); environments under
[DEPENDENCIES](DEPENDENCIES.md); release artifacts under
[RELEASES](RELEASES.md).

**Goal:** a clean, secure and maintainable repository - not the smallest possible one.
Never remove source, tests, legal files or reproducibility information to reduce size.

## 1. What normally belongs in Git

Track what the project needs for development, testing, functionality, building,
packaging configuration, maintenance, documentation, licensing, scientific
reproducibility and CI:

- application source code and required assets, including official logo and icon
  resources;
- build scripts and installer **source** (e.g. Inno Setup scripts, WiX sources,
  packaging specs);
- dependency manifests and the project's lockfiles;
- automated tests and the fixtures they need;
- CI workflows;
- README and relevant technical documentation, CHANGELOG, LICENSE, required third-party
  notices, security documentation;
- scientific methods, parameters and reproducibility metadata;
- configuration templates with placeholders (never real secrets).

Follow the project's own requirements; this list is not a reason to create files a
project does not need.

## 2. What normally stays out of Git

Use ecosystem-appropriate rules, checked against the actual project:

| Category | Examples | Keep in Git instead |
| --- | --- | --- |
| Development environments | `.venv/`, `venv/`, Conda environment directories, `env/` **only when it is a generated environment**, local interpreters, temporary toolchains | environment definitions (`pyproject.toml`, `environment.yml`, `renv.lock`, toolchain files) |
| Dependency installations | `node_modules/`, package and download caches | manifests and lockfiles |
| Build outputs | compiler output, generated executables, copied DLLs or shared libraries, packaging work directories, generated installers and archives | build scripts and installer sources; binaries only where tracking them is the project's established design |
| Temporary and diagnostic files | runtime logs, temporary files, crash dumps, debug output, coverage reports, generated test results, throwaway screenshots | intentional test fixtures, reference outputs and reproducibility evidence |
| Operating-system and IDE files | `.DS_Store`, `Thumbs.db`, editor swap files, machine-specific IDE caches and workspace state | portable, intentionally shared project configuration (e.g. a tracked `.editorconfig` or shared run configurations) |
| Confidential files | credentials, private keys, tokens, `.env` files with real values, personal databases, sensitive research data, private documents, machine-specific confidential configuration | templates with placeholders |

Never ignore a legitimate source directory merely because of its name (an `env/` package,
a `build/` module, a `logs/` feature). `.gitignore` is not a security boundary: it does
not protect files that are already tracked, force-added or copied elsewhere - secrets
also need the safeguards in [SECURITY](SECURITY.md).

## 3. Scientific data and large files

Distinguish source code, small test datasets, published reference resources, raw
sequencing data (FASTQ), BAM/CRAM alignments, large microscopy images, generated genome
indexes, analysis outputs and reproducibility manifests. Large experimental files are
not pushed automatically: use the project's established route - a data repository or
archive (with accession numbers), artifact storage, or Git LFS where justified - and keep
small representative test data in the repository when tests need it. Preserve metadata
and provenance (checksums, versions, accessions) so the analysis stays reproducible; do
not delete reproducibility information to save space.

## 4. Installer and release files

Installer **sources** usually belong in Git; generated installers and packages
(`Setup.exe`, MSI, DMG, PKG, DEB, RPM, AppImage) and intermediate build directories usually
do not - they are release artifacts, published only through an authorized release
([RELEASES](RELEASES.md)). Never delete locally generated installers
just because they must not be committed; they may be needed for validation or
distribution.

## 5. `.gitignore` changes

Inspect the project before editing: languages, build systems, dependency managers,
generated directories, intentionally tracked assets and sensitive local files. Then:
keep existing valid rules; add narrow, anchored rules (`/build/` rather than `build`
when only the root output directory is meant); avoid broad patterns that could hide
source; document deliberate exceptions (`!` rules) with a comment; and check that no
essential file is now ignored (`git check-ignore -v <path>`, `git status --ignored`).
Never replace a project's `.gitignore` wholesale with a generic template.

## 6. Files already tracked

Ignoring does not untrack. For unnecessary tracked files: identify the exact paths,
confirm they are unnecessary (contents, references from code, build and tests), remove
them from the index only (`git rm --cached <path>`), keeping local copies where useful,
add a matching ignore rule, and review the staged result. Never run broad destructive
cleanup (`git clean -fdx`, recursive deletes, extension-based mass removal), and never
remove files from published history without explicit authorization.

## 7. Staging discipline

Before committing: review `git status`; identify the intended changes, unrelated
modifications, generated files, possible secrets, large files and any required file that
is missing; stage intended paths explicitly; review `git diff --cached`; verify the
commit contents. Never assume untracked files belong in the commit, never stage
unrelated user work to obtain a clean tree, and never commit with blanket `git add -A`
or `git commit -a` without reviewing the result.

## 8. Structure

Keep the project's organisation. Do not reorganise directories for appearance, and do
not create duplicate documentation, build, installer, configuration, manifest, policy or
test locations. Introduce a new directory only when the functionality needs it.

## 9. Local development files

Environments, downloaded tools, caches and temporary working files stay outside tracked
content. Keep apart: the local environment, repository sources, required application
assets and release contents. Never commit a whole virtual environment, personal
absolute paths, or AI-agent configuration and session data (agent instruction files,
transcripts, local agent settings) unless the project deliberately tracks such a file.

## 10. The portable gateway

`.ai-policy/POLICY.md` is permitted minimal development metadata: never classify it as
unnecessary or remove it automatically, and never copy the central policy framework into
an application repository beside it. It is not a build or runtime input, is excluded from
installers and runtime bundles, and - when tracked - appears in GitHub's generated source
archives ([gateway](../gateway/POLICY.md)).

## 11. Before pushing

In addition to [DEVELOPMENT_POLICY](../DEVELOPMENT_POLICY.md) section 6: review tracked and
untracked files; identify generated outputs, unnecessary large files and accidental
environments; check that essential files are still present (sources, build and installer
sources, tests and fixtures, manifests and lockfiles, legal notices, scientific
provenance, required resources, CI configuration, reproducibility documentation); check
dependency changes and required notices. Never delete or untrack files based only on
broad extension rules. Automated checks help but do not replace review.

## 12. After pushing

Confirm that the expected files arrived and nothing was omitted, in addition to
[POST_PUSH_VERIFICATION](POST_PUSH_VERIFICATION.md). If something unintended was pushed,
follow [SECURITY](SECURITY.md) section 3 (accidental publication).
