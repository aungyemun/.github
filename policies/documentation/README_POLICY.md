# README Policy

A README tells a new reader, quickly and accurately, what the software is, what state it
is in and how to start. It summarises and links to the other documents rather than
repeating them. Apply it with the [General Writing Policy](GENERAL_WRITING_POLICY.md) and
adapt it to the software's purpose and maturity; no section is mandatory.

## 1. Content

Choose by purpose, maturity and evidence: name and one-sentence purpose; short overview;
verified version and status; key **implemented** features; screenshots for graphical
applications; supported operating systems (only those built or tested); system
requirements; installation or a link to it; a minimal working usage example; build,
development and test pointers; documentation navigation; known limitations; licence
(matching LICENSE); support and security references when they exist.

Never invent features, releases, versions, tests, screenshots, performance results,
compatibility claims, contacts or links, and avoid marketing language.

## 2. Version and release status

Show a version only when it is useful and verified. A commit or branch is not a release.

| Repository | What to show |
| --- | --- |
| Published releases | The latest stable version, checked against GitHub Releases, tags and release metadata, linked to the real release page, with the platforms that have packages; prereleases labelled as such |
| Active development | `Status: Active development`; a development version only if the project's version metadata defines it; no "latest release" wording or links to releases that do not exist |
| Experimental / prototype | `Status: Experimental` or `Status: Prototype`; a version is optional |
| Archived | Archived status, last verified published version, real maintenance status |
| Library / source-only | The project's convention (often a package version); distinguish source, package and release versions |

Place version and status near the top and keep release history in CHANGELOG.md. Before
writing a version, check version metadata, tags, GitHub Releases, the changelog and the
published artifacts; if they disagree, raise it instead of picking the highest number.

Status terms, used only with evidence: Experimental, Prototype, Active development,
Alpha, Beta, Release candidate, Stable (designated stable by the maintainer),
Maintenance, Archived. Do not infer stability from a version number or production
readiness from unit tests, do not call a development branch a release, do not call a
quiet project abandoned, and do not invent maintenance guarantees.

## 3. Public and private repositories

Determine visibility from repository metadata, not from text in the repository.

- **Private repositories:** focus on purpose, implementation status, setup, build, tests,
  limitations and existing technical documents. They may still hold real releases; do
  not add or remove release information automatically.
- **Public repositories:** give visitors an overview, features, accurate status,
  platforms, installation, usage, documentation links, licence and support/security
  information. Remove personal paths, internal infrastructure, confidential information
  and links visitors cannot open; verify download URLs.
- **Before making a repository public:** review name, description, version, status,
  downloads, platforms, screenshots, links, licence and attributions, security references
  and whether outsiders can follow the instructions; inspect source **and history** for
  secrets and confidential material (deleting a file does not remove it from history).
  Visibility changes need the maintainer's explicit decision.
- **After a repository becomes private:** keep accurate content; update only claims that
  became wrong.

## 4. Screenshots

Recommended for graphical applications; optional for CLIs, libraries, services and early
prototypes.

- Screenshots come from a real build of an identified version, show implemented
  features, and contain no personal or sensitive data, misleading crops, nonexistent
  controls or fabricated results. Captions match what is shown; outdated screenshots are
  replaced.
- Generated or rendered images are never presented as screenshots; conceptual
  illustrations are labelled as such.
- Process: launch the real application with safe demo data, size it for readability,
  capture, check for sensitive content, crop OS chrome, optimise without blurring text,
  name the file descriptively and add meaningful alt text.
- Formats: PNG for UI, WebP where useful, JPEG for photos, SVG for original vector art.
- Location: an existing asset folder, otherwise `docs/images/` (or `assets/screenshots/`);
  never in source directories; never packaged into application releases.
- Placement: after the introduction, status and key features (one overview image may come
  directly after the status). Guide: 1-2 images for small utilities, 2-4 for moderate and
  3-6 for complex applications; larger sets go to a gallery or user guide.
- Markdown: `![Main application interface](docs/images/main-interface.png)`; simple
  `<img src="..." alt="..." width="800">` only when a width is needed; repository-relative
  paths, no external image hosting.

## 5. Logos, icons and badges

- **Logos:** only the official branding from original assets, readable in light and dark
  mode, moderately sized; no new or unlicensed logos without the maintainer's decision
  (creating and adapting logos: [LOGO_AND_BRANDING](../LOGO_AND_BRANDING.md)).
- **Icons:** only where they aid navigation or comprehension, with text labels, one
  consistent licensed style, no emoji walls.
- **Badges:** optional and few (release, CI, licence, package version); each shows real
  data and links to an existing destination that matches the repository.

## 6. GitHub alert callouts

Use the five GitHub alert types selectively: `NOTE` (supplementary information), `TIP`
(practical suggestions), `IMPORTANT` (essential information), `WARNING` (significant
risks), `CAUTION` (risk of damage or data loss).

```markdown
> [!IMPORTANT]
> Back up your existing data before performing a major upgrade.
```

Not for ordinary paragraphs or decoration; never exaggerate risk; keep them accurate.

## 7. Diagrams and graphs

- **Diagrams** only when they explain better than prose; keep README diagrams small,
  prefer Mermaid, match the actual implementation, check that they render, and move
  complex diagrams to technical documents.
- **Graphs** only with authentic data, stated measurements, units and conditions, honest
  axes, clear separation of illustrative and measured results, and conclusions the data
  supports. Never fabricate comparisons; link extensive figures to dedicated reports.

## 8. Documentation navigation

Add a Documentation section listing only documents that exist, with relative links and
descriptive text:

```markdown
## Documentation

| Document | Description |
| --- | --- |
| [Installation](INSTALLATION.md) | Installation and setup instructions |
| [Building from source](BUILD.md) | Development dependencies and build steps |
| [Changelog](CHANGELOG.md) | Version history |
```

Group links (user, developer, release and compliance, scientific) only in large
repositories; omit empty groups. Avoid unnecessary absolute GitHub URLs and broken
anchors. Summaries in the README must match the linked documents.

## 9. Layout, length and accessibility

- Clear heading hierarchy, short paragraphs, few badges, useful tables, minimal
  decoration, readable on mobile and in light and dark mode; little HTML.
- Released desktop application, adaptable order: logo, name, description, version /
  status / platforms, overview, key features, preview, platforms, requirements,
  installation, quick start, documentation, building, testing, privacy and security,
  limitations, contributing, releases, licence, acknowledgements, support. CLIs replace
  the preview with a usage example; scientific software adds methods or reproducibility.
- Development repository: name, purpose, status, implemented features, setup, usage,
  building, testing, limitations, documentation, licence.
- Length guide (not a limit): about 50-150 lines for small utilities, 100-300 for
  moderate and 150-500 for mature applications.
- Accessibility: meaningful alt text, information not conveyed by colour or icons alone,
  real headings in order, tables with header rows, descriptive link text.

## 10. Verification

Before committing, check name, version, status, visibility, platforms, installation and
download links, features, screenshots (authenticity, version, sensitive content, paths),
icon and logo licences, badge targets, Mermaid syntax, graph data, documentation links,
rendering, privacy/security/licence statements and consistency with other documents.
Report only the checks actually performed.

After pushing, view the rendered README on GitHub (text, images, links, badges, alerts,
diagrams) and check CI. Correct problems with a normal follow-up commit, never by
rewriting published history.
