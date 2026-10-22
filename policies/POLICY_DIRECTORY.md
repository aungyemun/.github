# Central Policy Directory

A navigation aid for people and AI assistants: what each policy covers, when it applies,
and where to start for common short instructions. It adds no rules.
[DEVELOPMENT_POLICY](../DEVELOPMENT_POLICY.md) remains authoritative for precedence and
for choosing policies, and explicit maintainer instructions and repository requirements
still come first.

**How assistants use it:** use this directory only when the development policy's
selection table does not already name the policies a task needs; then read only the
selected policies. A trivial operation does not require reading this page.

## Engineering policies

Engineering policies say **what to do**. Policies marked *conditional* apply only when the
stated condition holds.

| Policy | Responsibility | Typical tasks | Applies |
| --- | --- | --- | --- |
| [DEVELOPMENT](DEVELOPMENT.md) | understanding before changing, focused compatible changes, architecture, reviewing generated work | any code change | always for code changes |
| [DEPENDENCIES](DEPENDENCIES.md) | dependency selection and versions; local development environments, external tools, licences of components | adding packages, preparing or repairing an environment, running the app | when dependencies or environments are involved |
| [SECURITY](SECURITY.md) | secrets, repository and CI security, application security, supply chain, updates, vulnerabilities | security reviews, network or update features, pushes | always for credentials; otherwise by risk |
| [PRIVACY](PRIVACY.md) | data classification, offline terminology, network, telemetry, diagnostics, storage, retention | features handling user or research data, network code | when personal, confidential or research data is involved |
| [PERFORMANCE](PERFORMANCE.md) | correctness-first optimisation, resources, responsiveness, benchmarking | lag, memory use, large data, optimisation | when speed or resources matter |
| [UI_UX_DESIGN](UI_UX_DESIGN.md) | interface structure, typography, multilingual text, colour, layout, accessibility, licence display | UI changes, About pages, multilingual interfaces | conditional: graphical interfaces only |
| [LOGO_AND_BRANDING](LOGO_AND_BRANDING.md) | reusing branding, logos only when asked, platform icon assets | logos, application icons | conditional: visual identity work |
| [TESTING](TESTING.md) | risk-based test strategy, test quality, regression tests, evidence and reporting | any change that needs verification | always, proportionately |
| [TEST_DATA_AND_FIXTURES](TEST_DATA_AND_FIXTURES.md) | choosing and creating test materials, expected results, fixture storage and provenance | creating test materials, golden files | when tests need data |
| [TROUBLESHOOTING](TROUBLESHOOTING.md) | root-cause investigation, diagnostic escalation, logging and tracing, release diagnostics | bugs, crashes, logging, tracing | for defects and diagnostics |
| [REPRODUCIBILITY](REPRODUCIBILITY.md) | independence from these development policies, reproducible builds and analyses | builds, scientific analyses | always for independence; otherwise by project |
| [SCIENTIFIC_PROVENANCE](SCIENTIFIC_PROVENANCE.md) | implemented methods, verified citations, genome and annotation provenance | scientific pipelines, reference updates, methods | conditional: scientific or research software only |
| [INSTALLER_AND_PACKAGING](INSTALLER_AND_PACKAGING.md) | stable identity, in-place upgrades, user-data protection, shortcuts, installer tests | installer or packaging work | conditional: applications with installers or packages |
| [VERSION_MANAGEMENT](VERSION_MANAGEMENT.md) | version schemes, bump rules, platform version histories, native metadata | version bumps, new platform packages | when versions change |
| [RELEASES](RELEASES.md) | release checks, artifact inspection and exclusion, publication controls | preparing or publishing a release | conditional: established release process and a release operation |
| [REPOSITORY_HYGIENE](REPOSITORY_HYGIENE.md) | what belongs in Git, `.gitignore`, untracking, staging discipline | cleaning a repository, preparing a push | for commits and repository cleanup |
| [GIT_OPERATIONS](GIT_OPERATIONS.md) | authorization for Git operations, commit messages, pushing, failure handling | commits, pushes, merges | for Git operations |
| [POST_PUSH_VERIFICATION](POST_PUSH_VERIFICATION.md) | verifying the remote, CI, releases and commit messages after a push | after every push | after pushes |

## Documentation policies

Documentation policies say **how to write and verify** the corresponding documents. Start
with the [Documentation Policy Index](documentation/DOCUMENTATION_INDEX.md): it lists every
documentation policy, when each applies, and which engineering policy supplies the facts.
The [General Writing Policy](documentation/GENERAL_WRITING_POLICY.md) applies to all
documentation work; document-specific policies (README, CHANGELOG, BUILD, INSTALLATION,
TESTING, SECURITY, PRIVACY, METHODS, THIRD_PARTY_NOTICES, RELEASE and others) apply only
when that document is created or changed.

## Common short instructions

Mappings come from the development policy's
[choosing policies](../DEVELOPMENT_POLICY.md#choosing-policies-for-a-task) table (named in
the second column); if the two ever differ, the development policy wins. Conditional
supporting policies are loaded only when the condition stated there holds.

| Short instruction | Development policy row | Primary | Supporting |
| --- | --- | --- | --- |
| "Fix bugs." | Fixing a bug | TROUBLESHOOTING | DEVELOPMENT, TESTING |
| "Improve the UI." | Improving a user interface or a multilingual interface | UI_UX_DESIGN | DEVELOPMENT, TESTING |
| "Update README." | Updating a README or changelog | README_POLICY | GENERAL_WRITING_POLICY; CHANGELOG_POLICY for changelog updates |
| "Prepare installer." | Installer, packaging or release preparation | INSTALLER_AND_PACKAGING | VERSION_MANAGEMENT, TESTING; RELEASES with an established release process |
| "Create testing materials." | Testing the application; creating test materials | TEST_DATA_AND_FIXTURES | TESTING |
| "Improve performance." | Lag, freezes, high CPU or memory use | PERFORMANCE | TROUBLESHOOTING, TESTING |
| "Investigate crashes." | Finding an underlying cause; adding logging or tracing | TROUBLESHOOTING | TESTING, PRIVACY |
| "Update scientific methods." | Updating `METHODS.md` | METHODS_POLICY | SCIENTIFIC_PROVENANCE |
| "Update third-party notices." | Updating third-party notices | THIRD_PARTY_NOTICES_POLICY | DEPENDENCIES, LICENSE_POLICY |
| "Clean repository." | Cleaning the repository or changing `.gitignore` | REPOSITORY_HYGIENE | GIT_OPERATIONS; REPRODUCIBILITY when data or environments are involved |
| "Prepare GitHub push." | Preparing a push | REPOSITORY_HYGIENE | GIT_OPERATIONS, SECURITY, TESTING |
| "Prepare application release." | Installer, packaging or release preparation | RELEASES | INSTALLER_AND_PACKAGING, VERSION_MANAGEMENT, TESTING |

## What decides applicability

Policy selection depends on the actual project and task: project type, programming
language, target operating systems, supported interface languages, functionality, the
current task, existing repository conventions, and the release or development state.
Scientific policies are not applied to unrelated applications, installer policies not to
repositories without a distribution process, UI policies not to command-line-only
projects, and release rules only to release operations in repositories with an
established release process.

## Machine-readable inventory

[`policy-manifest.json`](../policy-manifest.json) lists every policy file (with hashes), the
topic name of each engineering policy and the location of this directory.
