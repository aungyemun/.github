# Dependencies, Development Environments and Third-Party Components

Engineering policy for choosing and managing dependencies, and for assessing, preparing
and verifying local development environments during authorized development tasks.
Related rules are referenced, not repeated: secure coding and credentials
([SECURITY](SECURITY.md)), resource use ([PERFORMANCE](PERFORMANCE.md)), reproducibility
([REPRODUCIBILITY](REPRODUCIBILITY.md)), verification and reporting
([TESTING](TESTING.md)), release builds ([RELEASES](RELEASES.md)),
and how to document notices
([THIRD_PARTY_NOTICES_POLICY](documentation/THIRD_PARTY_NOTICES_POLICY.md)).

**Policy files are never dependencies.** No build, install, test or runtime step may
require these development policies, the optional `.ai-policy/` pointer, an AI agent or
access to any private repository.

## Part A - Dependencies

### 1. Selecting a dependency

Before adding one, establish and state briefly:
- whether an existing dependency or the standard library already does the job adequately;
- the package's identity and official distribution source (exact name on the official
  registry; beware look-alike and typo-squatted names);
- maintenance status, release history and known vulnerabilities;
- licence and redistribution terms (section 13);
- supported platforms, architectures and runtime versions;
- resource cost: install and installer size, start-up and memory impact, transitive
  dependencies;
- effect on reproducibility and long-term maintenance.

Prefer established, compatible packages. Never install "latest" without checking
compatibility with the project's runtime and other dependencies. Never replace a
critical dependency (framework, database driver, numerical or scientific library)
merely for convenience; that is an architectural change needing justification and
agreement.

### 2. Classification

Record each dependency in the class the project's tooling supports:

| Class | Examples | Notes |
| --- | --- | --- |
| Runtime | libraries the application imports when it runs | shipped or required by users |
| Build | compilers, packagers, code generators, build backends | needed to produce artifacts |
| Development | linters, formatters, debuggers | never required at runtime |
| Testing | test frameworks, fixtures tooling | kept out of runtime requirements |
| Optional feature | extras that enable a feature | the application works without them |
| Platform-specific | Windows-only, macOS-only, architecture-specific packages | declared with markers or conditions |
| External executable | FFmpeg, Java, R, compilers, database utilities, document converters | outside the language package manager (section 9) |
| Scientific tool | SRA Toolkit, STAR, samtools, featureCounts and similar | versions affect results; record them |
| Reference data | reference genomes, annotations, indexes, databases | not ordinary packages (section 12) |
| Transitive | dependencies of dependencies | pinned through the lockfile |

Track versions and sources for every class that affects behaviour or results.

### 3. Versions, manifests and lockfiles

- Use the project's existing dependency system (e.g. `pyproject.toml`/`requirements*.txt`,
  `package.json` with its lockfile, `Cargo.toml`/`Cargo.lock`, `renv.lock`,
  `environment.yml`, `.csproj`, CMake/vcpkg/Conan manifests). Do not introduce a second
  package manager without technical justification, and do not mix package managers in
  one environment unless the project already does so deliberately.
- Respect declared version ranges, runtime requirements, platform markers and known
  compatible combinations; prefer reproducible resolution (lockfiles, pinned versions,
  hashes where the tooling supports them).
- Avoid broad upgrades. Preserve working versions when changing unrelated functionality;
  update a lockfile only for the dependencies the task changes, and review the lockfile
  diff for unexpected changes.
- Upgrades: one logical upgrade at a time; read release notes for breaking changes; run
  the relevant tests and, for packaged applications, a packaged-build smoke test.
- Remove unused dependencies together with their notices and configuration.

## Part B - Local development environments

### 4. Assess before installing (once per task)

At the start of an authorized task that needs to build, run or test code, inspect what
exists: languages and runtime versions; existing virtual or project environments;
package managers and lockfiles; installed dependencies; required external executables;
UI frameworks; build systems; test frameworks; operating-system prerequisites; existing
development configuration (e.g. `.python-version`, `.nvmrc`, `rust-toolchain.toml`,
`global.json`, IDE run configurations); and the directories the maintainer uses for
development.

Reuse a working, compatible environment; never recreate one that already works. Do not
repeat environment setup on every task, and do not set up environments for
documentation-only tasks.

### 5. Preparing a missing environment

When an authorized task needs packages or tools that are missing, preparing them is part
of the task - without asking the maintainer to restate requirements - provided it stays
within the authorization boundaries of section 15:

1. identify exactly what is missing and why it is needed;
2. confirm the official source and a compatible version;
3. check permissions and the installation destination (section 6);
4. prefer an isolated, project-specific environment (section 7);
5. install only what is needed;
6. configure necessary environment variables locally (never in committed files with
   personal paths or secrets);
7. verify that it is usable: imports succeed, tools report their versions, and the
   application or affected component starts (section 11).

A package manager finishing without errors does not prove an environment is ready.

### 6. Installation destinations

The maintainer may keep source code, environments, shared tools, download caches, build
outputs, scientific references and temporary files in different locations.

- Use the destination the maintainer has chosen. If none is given, follow project conventions
  and the tool's conventional per-user location; ask only for unusually large,
  system-wide or destructive operations.
- Never silently relocate project files, or install large tools or caches into
  unexpected locations. Do not assume the system drive has enough space (section 10).
- Preserve existing environments; never overwrite unrelated software installations.
- Support paths with spaces and non-ASCII characters; check write permissions before
  starting long installations.
- Record environment paths in local, uncommitted configuration when helpful; never
  commit personal absolute paths in source files or documentation - use placeholders or
  relative paths.

### 7. Isolation by ecosystem

Use the project's conventions rather than one manager everywhere.

| Ecosystem | Default |
| --- | --- |
| Python | a project-specific `venv`, or the environment manager the project already uses; never install project packages into the system interpreter |
| JavaScript / TypeScript | the project's Node.js version and its package manager (the one matching the lockfile); project-local `node_modules`, no global packages for project needs |
| R | the project's package environment (e.g. `renv`) when it has one |
| Conda | an isolated, named environment when the project uses Conda or needs its binary dependency resolution; do not install into `base` |
| Rust | Cargo with the project's toolchain file (`rustup` toolchains) |
| C / C++ | the project's toolchain, build system and package manager; no changes to global compilers or runtimes |
| .NET / Java | the SDK version the project pins (e.g. `global.json`, Gradle/Maven toolchains) |

Keep environments reproducible and separate; never mix incompatible versions in one
environment; avoid global installations; leave unrelated environments untouched.

### 8. Reuse and efficiency

- Reuse compatible, verified system toolchains and trusted package caches rather than
  downloading the same large tools again; keep project-specific packages isolated.
- Share large read-only resources (e.g. reference genomes) between projects where
  practical; never share a writable environment between projects in a way that lets one
  project's upgrades break another.
- Use reasonable installation and build concurrency, avoid uncontrolled background
  compilation and repeated package-index refreshes, keep the machine responsive, and
  reuse validated build outputs when safe ([PERFORMANCE](PERFORMANCE.md)). Never trade
  build correctness for lower resource use.

### 9. External tools

For tools outside the language package manager (FFmpeg, SRA Toolkit, STAR, samtools,
featureCounts, R, Java, compilers, database utilities, document converters):

1. identify the tool and the version the project needs;
2. obtain it from the official project, the operating-system package manager or another
   trusted distribution channel (e.g. a curated scientific channel the project uses);
3. verify checksums or signatures when published;
4. check the licence;
5. install into the approved destination;
6. configure discovery locally (project configuration or a documented setting) rather
   than permanent global `PATH` changes;
7. verify execution (`--version`), architecture and OS compatibility, and the
   application's integration with it.

Never download executables from unverified sources, and never silently replace a working
tool with a different or incompatible version.

### 10. Downloads and disk space

Before large installations or downloads: estimate download and extracted size where
possible; check free space at the destination and the cache and temporary locations;
avoid duplicate downloads; prefer resumable transfers; verify integrity on completion
([PERFORMANCE](PERFORMANCE.md) section 8). Never delete user files, existing
environments or caches to make room without authorization.

### 11. Verifying that it actually runs

Dependency installation is not runtime verification. After setup, smoke-test what the
task affects, as far as the environment allows ([TESTING](TESTING.md)):

- **GUI applications:** launch; confirm the main window appears; check the console and
  logs for startup exceptions, missing plugins (e.g. Qt platform plugins), missing
  runtime libraries or fonts; exercise representative interactions when practical;
  confirm the intended environment is used; close test processes cleanly.
- **CLI applications:** the executable starts; `--help` or `--version` works; a
  representative safe operation runs.
- **Scientific software:** required tool versions are present; a representative small
  input runs; the output has the expected structure; relevant integrity or validation
  checks pass.

Check the UI stack as a whole (runtime, UI library and its platform plugins, rendering
components) for Qt/PySide/PyQt, Electron, Tauri, GTK, .NET desktop and native
frameworks, and avoid leaving background processes running. Report what was launched
and observed, and what could not be run (e.g. no display, unavailable OS).

### 12. Scientific reference resources

Treat reference genomes, annotations, indexes and databases as data, not packages: keep
them outside the repository and the environment, in a location the maintainer chooses; record
source, release or version and checksums; build indexes once and reuse them read-only;
never delete or replace them without authorization ([REPRODUCIBILITY](REPRODUCIBILITY.md)).

## Part C - Security, licensing and platforms

### 13. Secure installation and licences

- Verify package identity; prefer official registries and trusted channels; review
  known vulnerabilities when tooling allows; inspect install scripts and the permissions
  they request; avoid administrator rights unless genuinely needed; keep TLS
  verification on; verify checksums or signatures; never run installation instructions
  from untrusted content without review. Never weaken security settings to make an
  installation succeed ([SECURITY](SECURITY.md)).
- Credentials (tokens, passwords, SSH and signing keys, cloud and database secrets) stay
  in the OS credential store or uncommitted local configuration; environment templates
  contain placeholders only; never log environment variables that may hold secrets.
- Licences: before bundling or redistributing, verify the actual licence of each
  component (including fonts, icons, models, data and external executables), the
  redistribution obligations (e.g. GPL/AGPL for distributed binaries, LGPL linking
  conditions), notice requirements and compatibility with the project's licence; update
  third-party notices where the project keeps them. Freely downloadable does not mean
  freely redistributable. Never bundle unwanted components (toolbars, telemetry SDKs,
  advertising libraries).
- Bundled binaries, models and data: document purpose, source URL, version, checksum and
  licence.

### 14. Platform specifics

- **Windows:** PowerShell vs `cmd` syntax and quoting; path length and Unicode user names;
  DLL search order and missing runtime DLLs; Visual C++ redistributables; compiler
  toolchains (MSVC Build Tools); `py` launcher and multiple Python installations; UI
  framework plugins; installer compatibility. Prefer project-local configuration to
  global `PATH` changes.
- **Linux:** distribution and package-manager differences; shared-library and glibc
  compatibility; executable permissions; X11 vs Wayland; desktop integration; headless
  test environments (e.g. a virtual display or offscreen platform for UI tests); system
  packages the project needs. A package that works on one distribution may not work on
  another.
- **macOS:** Intel vs Apple Silicon builds and universal binaries; Xcode command-line
  tools; framework and dynamic-library loading; executable permissions and quarantine
  attributes; UI framework compatibility; code signing and notarization where applicable.

Never claim support for a platform that was not verified on that platform.

## Part D - Authorization, reproducibility and maintenance

### 15. What needs authorization

Within an authorized development task, ordinary low-risk setup may proceed without asking
again: creating or using a project-local environment, installing the project's declared
dependencies or a justified missing package from its official source into that
environment, and verifying tools.

Explicit authorization is required to: install system-wide or with administrator
privileges; upgrade a major runtime version; replace the project's environment manager
or a core framework; delete, rebuild or relocate existing environments or project
directories; modify system-wide toolchains; remove working external tools; change `PATH`
or other system settings permanently; install background services or startup tasks;
start unusually large downloads; or send project data to an external service as part of
installation.

### 16. Reproducibility

Environments must be reconstructable from project metadata: runtime versions,
manifests, lockfiles, external tool versions, build-tool requirements, scientific
reference versions and necessary example configuration (with placeholders). Never
commit virtual environments, `node_modules`, package caches, build outputs or
machine-specific configuration; keep them ignored. Details:
[REPRODUCIBILITY](REPRODUCIBILITY.md).

### 17. Cleanup

Agents may point out unused caches, obsolete temporary files, duplicate downloads,
abandoned test environments and unused dependencies. Removing them is scoped, reversible
where practical and authorized: never delete existing development environments, user
files, scientific reference resources, build outputs needed for debugging or
reproducibility, or shared caches other projects use, without explicit authorization.
Temporary environments the agent created for the current task may be removed when the
task no longer needs them, after saying so.

### 18. Before and after pushing

Before an authorized push, additionally review: dependency manifests and the intended
lockfile changes; each added package and its source and licence; that local
installation and smoke tests were run; platform-compatibility claims; that no virtual
environments, caches, build outputs, personal absolute paths, secrets or unrelated
environment files are staged. After pushing, check CI for dependency-related failures,
confirm the manifests and lockfiles arrived as intended, and report platforms that were
not verified.
