# Changelog

All notable changes to these development standards are documented here. The format
follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/).

## [Unreleased]

## [1.4.0] - 2026-10-22

### Added
- Performance policy: correctness-first priority order, CPU, memory, disk, startup and
  idle behaviour, GUI responsiveness, concurrency, network, database, large scientific
  data, cross-platform differences, benchmarking method and regression prevention.
- Guidance on choosing the policies relevant to a task.
- Development-environment guidance in the dependency policy: assessment and reuse,
  setup and verification, installation destinations, isolation per ecosystem, external
  tools, UI stacks, downloads and disk space, reference data, platform specifics,
  authorization boundaries, reproducibility and cleanup.
- Installer, packaging and upgrade-safety policy: stable identity, detection of previous
  installations, in-place upgrades, safe file replacement, user-data protection,
  running-application and concurrent-installer handling, shortcuts and launchers,
  duplicate prevention, single-instance execution, uninstall registration, failure
  recovery, silent installation, platform technology and an installer test matrix.
- User interface and user experience policy: application-specific design, framework
  conventions, typography, multilingual fonts and complex text, alignment, spacing and
  colour, responsive layouts, Windows/macOS/Linux conventions, control organisation,
  accessibility, lightweight UI, copyright and licence presentation, verification.
- Logo, icons and branding policy: reuse of existing branding, creating logos only when
  asked, design principles, platform icon assets, variants, quality verification,
  provenance and licensing.
- Repository hygiene policy: what belongs in Git, exclusions by category, scientific
  data and large files, installer sources versus generated installers, `.gitignore`
  changes, safe untracking, staging discipline, structure, local files, the portable
  gateway, and pre-/post-push checks.
- Version management policy: precedence of explicit and established schemes, an optional
  four-component default for new applications (`0.0.0.1`), bump rules, independent
  platform version histories, native metadata mapping, artifact names, changelog and
  update compatibility.
- Scientific provenance policy for scientific software: implemented methods, verified
  citations, scientific software inventories, reference genome and annotation provenance
  (including Ensembl releases), official sources, analysis records, scientific changes,
  document roles, platform-specific distributions and verification; methods and
  third-party notices policies extended accordingly.
- Testing policy extended (risk-based levels, test-script quality, templates, bug-fix
  regression tests, cross-platform evidence); new test data and fixtures policy; new
  troubleshooting, logging and diagnostics policy (moved out of the development policy)
  with proportionate escalation, structured logging, workflow tracing, log privacy and
  retention, crash diagnostics and release-build decisions.
- Policy directory (`policies/POLICY_DIRECTORY.md`): every policy with its responsibility,
  typical tasks and applicability, a pointer to the documentation index, and common short
  instructions mirrored from the policy-selection table.
- Handling of brief instructions, and a reminder that guidance never widens
  authorization.

### Changed
- Privacy policy extended: data classification, collection and minimisation, offline
  terminology, network communication, telemetry, diagnostics, local storage, encryption,
  retention and deletion, research data, cross-platform checks and verification.
- Privacy documentation policy: recommended structure and writing rules.
- Security practices extended: security by design, secrets, detection and accidental
  publication, repository and GitHub Actions security, application security, supply
  chain, authenticated updates, installers and releases, diagnostics and vulnerability
  handling.
- Third-party notices and security documentation policies extended.
- Gateway pointer: discovery steps for AI assistants (visibility first, routing, load on
  demand, brief instructions, no implied authorization).

## [1.3.0] - 2026-10-22

### Added
- Changelog policy extended with versioning and release-status rules, unreleased work,
  public/private handling, platform-specific entries, breaking-change, security, privacy,
  scientific, performance and dependency guidance, presentation, links and historical
  integrity.

## [1.2.0] - 2026-10-22

### Added
- README policy extended with version and release-status rules, public/private guidance,
  screenshot authenticity and placement, logos/icons/badges, alert callouts, diagrams and
  graphs, documentation navigation, layout, accessibility and verification.

## [1.1.0] - 2026-10-22

### Added
- Commit-message standard: messages describe the change and contain no AI attribution
  (co-author trailers, session identifiers, conversation links, "Generated with ..."
  footers); Git identity, human co-authors and required disclosures are preserved.

## [1.0.0] - 2026-10-22

### Added
- Development policy with precedence rules, workflow, Git authorization and pre-/post-push
  verification.
- Engineering policies: development and troubleshooting, security, privacy, testing,
  dependencies, reproducibility, Git operations, releases, post-push verification.
- Documentation and professional writing standards (README, contributing, licence,
  security, changelog, installation and build, testing, privacy, third-party notices,
  scientific methods).
- Optional `.ai-policy/` gateway template and schema, project-profile index, and the
  intake format for reviewed lessons.
