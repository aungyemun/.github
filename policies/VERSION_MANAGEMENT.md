# Version Management

Engineering policy for application and platform versions: the optional four-component
scheme for new applications, bump rules, independent platform version histories, the
shared application version, version sources, native platform metadata, artifact naming,
changelog consistency, update compatibility and authorization. Release operations are in
[RELEASES](RELEASES.md); installer identity and upgrades in
[INSTALLER_AND_PACKAGING](INSTALLER_AND_PACKAGING.md); changelog writing in
[CHANGELOG_POLICY](documentation/CHANGELOG_POLICY.md).

## 1. Precedence

1. Explicit maintainer instructions about versioning (within safety and compatibility limits).
2. The project's established scheme and documented conventions.
3. The ecosystem's required format (PEP 440, npm/Cargo SemVer, Debian, RPM, Apple, MSI).
4. The framework default in section 3 - only for genuinely new applications.

Never impose the default on a project that already has a scheme.

## 2. Inspect before applying any rule

Check version metadata (project files, version resources, installer scripts), Git tags,
GitHub Releases, the CHANGELOG, existing installer packages and development builds,
release branches, version scripts and project documentation. A repository is not "new"
merely because it has no GitHub Releases: existing development or test versions count.
Never reset an existing version sequence. If the current version or the intended version
domain cannot be established, say so instead of inventing one.

## 3. Default for genuinely new applications

For a new general-purpose application with no established scheme, the recommended
default is four components, starting at **`0.0.0.1`**:

`MAJOR.MINOR.FEATURE.REVISION`

Not applied automatically to: existing or previously published applications; projects
with development or test-release versions; libraries and packages whose ecosystem
prescribes a format (e.g. SemVer for npm and Cargo packages); projects with explicit
custom instructions or documented conventions.

## 4. Bump rules (four-component scheme)

| Change | Rule | Examples |
| --- | --- | --- |
| Small fix or refinement | increment REVISION | `0.0.0.1 → 0.0.0.2`; optional rollover `0.0.0.99 → 0.0.1.0` |
| Meaningful new functionality or substantial improvement | increment FEATURE, reset REVISION | `0.0.0.3 → 0.0.1.0`; `0.0.1.0 → 0.0.2.0` |
| Larger product step agreed by the owner | increment MINOR, reset lower parts | `0.0.7.4 → 0.1.0.0` |
| Major architecture change, broken compatibility or a major milestone | increment MAJOR only after an explicit release decision, with a compatibility and migration assessment | never an automatic jump to `1.0.0.0` |

Advancing FEATURE does not require passing through every REVISION. Classify by real
impact: a bug fix is not a feature change. Do not bump for every internal edit or for
documentation-only changes unless the project requires it.

## 5. Version domains: shared application vs platform packages

Keep apart:
- the **shared application version** (functionality of the common codebase),
- each **platform package version** (Windows, Linux, macOS, others),
- **platform build numbers**,
- the **source commit** (Git SHA or tag),
- the **publication status** (development, tested build, published release).

Platforms may have independent histories, for example Linux package `0.0.5.0`, Windows
package `0.0.0.1`, macOS not started. A new platform does not inherit another platform's
number because that platform started earlier, and starting a new platform never resets or
changes the others. A platform package number is not evidence of equivalent
functionality on another platform: record which shared application version or commit
each package was built from.

## 6. One authoritative source per domain

Use the project's established version source (e.g. `pyproject.toml`, `package.json`,
`Cargo.toml`, a version resource, release configuration). Record platform versions with
the smallest suitable mechanism (e.g. a field per platform in the existing configuration,
or the installer script itself); add a new manifest only when nothing existing fits.
Generate other references from the source where the build allows it, so independent
edits cannot create conflicting values, and keep every value auditable in Git.

## 7. Native metadata mapping

Each format has its own rules; never write a string into a field that does not accept it.

- **Windows:** the user-facing product version (string) may carry the four components;
  the numeric file and product version resources (`VERSIONINFO`) take four integers of
  at most 65535 each; Windows Installer (MSI) `ProductVersion` allows
  `major.minor.build` with at most 255, 255 and 65535, and ignores a fourth field when
  comparing versions for upgrades - so an MSI must encode the version to make upgrades
  compare correctly; MSIX uses four parts (the Microsoft Store requires the last to be 0).
  Version numbers are never installer identity: keep `AppId`/`UpgradeCode` stable
  ([INSTALLER_AND_PACKAGING](INSTALLER_AND_PACKAGING.md) section 1).
- **Linux:** Debian `[epoch:]upstream_version[-debian_revision]` compared with dpkg rules
  (`~` sorts before anything); RPM `Version` and `Release` (and `Epoch`) compared with
  rpm rules; AppImage has no package-manager comparison (record the version in the file
  name and desktop metadata); Flatpak releases in AppStream metainfo. Comparison rules
  differ between systems - check them for unusual strings.
- **macOS:** `CFBundleShortVersionString` and `CFBundleVersion` accept up to three
  period-separated integers, so a four-component version must be mapped (e.g.
  `MAJOR.MINOR.FEATURE` as the short version and a monotonically increasing build number
  as the bundle version) and the mapping documented; installer packages and update
  feeds follow their own fields. The bundle identifier never changes with the version.
- **Language ecosystems:** PEP 440 accepts four release components; npm and Cargo require
  SemVer `MAJOR.MINOR.PATCH` (a four-component application version needs a mapping or
  stays outside those fields).

## 8. Artifact names

Name artifacts with the platform's own version and architecture, following existing
project conventions where they exist, for example
`Application_0.0.0.1_Windows_x64_Setup.exe`, `Application_0.0.5.0_Linux_amd64.deb`,
`Application_0.0.0.1_macOS_arm64.dmg`. Do not rename published artifacts, and do not
assume every application produces every format.

## 9. Changelog and release documentation

Keep the history accurate: distinguish application changes, platform packaging changes,
platform-specific fixes, development versions, tested builds and published releases;
label platform-specific entries with their platform and package version. Local build
artifacts do not make a version published. Never overwrite historical release notes
([CHANGELOG_POLICY](documentation/CHANGELOG_POLICY.md)).

## 10. Updates and upgrades

Update mechanisms compare the version identity of the same platform and package only:
prevent accidental downgrades, keep the installed application identity, preserve
settings and data, keep upgrade detection correct, avoid duplicate installations, and
never compare a Windows version with a Linux version. Changing an existing application's
versioning model needs a migration plan (e.g. ensure the first version under the new
model still compares as newer on every platform).

## 11. Performing an authorized version bump

1. Determine the current version from the authoritative source.
2. Identify the version domain (shared application, which platform package, or both).
3. Classify the actual changes.
4. Determine the next version under the applicable scheme.
5. Update the authoritative metadata.
6. Synchronise dependent references (installer scripts, version resources, About
   dialog, documentation) - only within that domain.
7. Update the changelog.
8. Run the relevant tests and version-consistency checks.
9. Report the old and new versions and the files changed.

A version bump is not a release: tags and GitHub Releases require explicit authorization
([RELEASES](RELEASES.md)).

## 12. Typical requests

| Request | Behaviour |
| --- | --- |
| "Fix a small bug." | fix it; bump REVISION only if the project bumps per change |
| "Add major functionality." | assess the project's feature-version rule; propose the bump |
| "Prepare Windows installer." / "Prepare Linux package." | use that platform's own version history |
| "Begin macOS version." | start macOS package versioning only if none exists; leave other platforms unchanged |
| "Update existing application." | keep the existing convention |
| "Bump version." | resolve the domain from project context and the request; ask if it remains ambiguous; never invent a version |
