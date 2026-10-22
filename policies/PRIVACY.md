# Privacy and Data Handling

Engineering policy: how agents design, implement, inspect and test privacy-relevant
application behaviour. How to *write* a user-facing privacy statement is covered by
[PRIVACY_POLICY](documentation/PRIVACY_POLICY.md); credential handling and secure coding
by [SECURITY](SECURITY.md). Nothing here requires an application to contain a
`PRIVACY.md`.

## 1. Scope and proportionality

- Apply this policy when a task touches personal or confidential data, files the user
  opens or produces, storage, logging, diagnostics, network communication, third-party
  services, or a release. Apply it during design, implementation, troubleshooting,
  testing, release preparation and pre-push review of such changes.
- Do not impose privacy-related changes on unrelated tasks. Report a privacy problem you
  notice outside the task scope instead of fixing it unasked.
- Applicable law, contracts, ethics approvals and institutional rules outrank this
  policy. This policy is engineering guidance, not legal advice, and never a basis for
  claiming legal compliance.

## 2. Privacy by design

- **Minimise:** collect, transmit, duplicate and retain only what the feature needs.
- **Purpose-limit:** use data only for the application function it was provided for.
- **Local first:** prefer local processing and storage when it meets the requirement.
- **Least access:** limit which components, processes and services see sensitive data.
- **Secure defaults:** optional data sharing is off until the user enables it, unless the
  maintainer has decided otherwise and the behaviour is disclosed.
- **User control:** give users a practical way to see, change, export or remove what the
  application keeps, in proportion to what it keeps.
- **Lifecycle:** protect data from collection to deletion, including copies, caches,
  backups and logs.
- **Evidence:** verify actual behaviour before stating or documenting any privacy property
  (section 13).

## 3. Data classification

Classify by **actual content and context**, never by file extension or location: a CSV
can hold patient records; a PDF can be a contract; a "test" database can be a copy of
production data. When unsure, treat data as the higher class and ask.

| Class | Meaning | Typical examples |
| --- | --- | --- |
| **Public** | intended for anyone; disclosure causes no harm | published papers and datasets, open-source code, released documentation |
| **Internal** | not meant for publication; limited harm if disclosed | project notes, non-sensitive configuration, build logs without personal paths |
| **Confidential** | disclosure harms the user, their organisation or study | personal documents and PDFs, unpublished results, application databases with user content, images of people, diagnostic logs with paths/usernames/file names, institutional data |
| **Highly sensitive** | disclosure causes serious harm or breaches legal/ethical obligations | credentials, keys, tokens and authentication data; medical and patient information; identifiable human-participant data; genetic data linked to individuals; restricted datasets under data-use agreements |

Rules by class (Mandatory safeguard for Confidential and Highly sensitive):
- Never upload, paste or send Confidential or Highly sensitive data to an external
  service (online converters, paste sites, AI or translation APIs, issue trackers)
  without the maintainer's explicit authorization for that specific transfer.
- Never commit them to repositories, test fixtures, documentation examples, CI output or
  issues. Credentials are handled under [SECURITY](SECURITY.md).
- Tests use synthetic or properly de-identified data; derive fixtures from real data only
  with authorization, and check the result for residual identifiers (names, IDs, dates,
  free-text fields, image metadata, file paths).

## 4. Collection and minimisation

- Every collected item has a stated functional purpose; collect nothing "for later".
- Avoid persistent identifiers (device IDs, hardware fingerprints, account IDs) unless a
  feature requires them; do not combine data sets in ways that make users identifiable.
- Keep optional diagnostics and analytics separable from essential functionality: the
  application must work with them disabled.
- Never add telemetry, analytics, tracking or advertising on your own initiative or for
  development convenience. When the maintainer or verified project requirements call for
  data collection, implement exactly what was specified, with disclosure and controls
  (section 7).

## 5. Offline, offline-capable and connected applications

Use these terms precisely, in code reviews, reports and documentation:

| Term | Meaning |
| --- | --- |
| **Fully offline** | no application-initiated external network communication during normal operation |
| **Offline-capable** | core functionality works without connectivity; optional network features may exist |
| **Connected** | some features require network access |

- Do not call software "offline" or "fully offline" unless verified. Check direct
  network code **and** dependencies and frameworks: update checkers, crash reporters,
  embedded web views, remote fonts or CDNs, licence checks, cloud SDKs, map tiles,
  model or data downloads.
- For a fully offline application, verify where feasible: core functions work with the
  network disabled; no mandatory cloud sign-in; no unexpected uploads; no hidden
  telemetry; no unnecessary connections; no runtime dependence on these development
  policies.
- For an offline-capable application, preserve authorized optional features (update
  checks, user-initiated downloads, sync). Do not remove legitimate network functionality
  merely to make the software qualify as offline.

## 6. Network communication

For every external communication a change adds or alters, establish and record (in the
change description or design notes):

1. why it is needed;
2. which destination (host, service, operator) receives data;
3. exactly what is sent, including metadata (file names, paths, versions, identifiers);
4. transport protection (TLS with certificate verification - never disabled to "fix" an
   error);
5. whether authentication is needed and how credentials are stored ([SECURITY](SECURITY.md));
6. whether the user initiates, controls or can disable it;
7. failure handling (timeouts, offline behaviour, no data loss, no silent retries that
   resend sensitive data);
8. what sensitive data could be exposed, including in URLs, query strings, headers and
   server logs (never put personal data in URLs).

Review update services, download services, cloud APIs, error-reporting endpoints,
authentication providers, synchronisation and third-party integrations with the same
questions. Communication must be transparent, purpose-limited and secured.

## 7. Telemetry and analytics

When telemetry is required:
- list every collected field and its purpose; collect the minimum, aggregated where
  possible;
- disclose it to users before collection and provide consent or an off switch
  appropriate to the data and the project's obligations;
- transmit securely; define retention; review each third-party recipient and what it
  receives;
- keep essential operational logs (local, needed for the application to function or be
  supported) distinct from optional usage analytics.

Never introduce telemetry silently. Never state that telemetry is absent or disabled
without checking the code, configuration **and** bundled SDKs, and, where feasible,
observing runtime behaviour.

## 8. Error reporting and diagnostics

Design diagnostics to be useful with the least personal information. Review payloads,
logs and crash dumps for: local paths, usernames, machine names, document and file names,
database identifiers, credentials and tokens, configuration values, stack-trace contents,
user-generated content, scientific data and memory dumps.

- Prefer error codes, component names, versions and sanitised stack traces; redact or
  hash sensitive fields; never send complete documents or datasets.
- Keep reports local by default. Automatic submission is never enabled silently; the user
  sees or can review what is sent and can decline. Protect submissions in transit and
  limit retention.
- Keep enough information to debug: do not strip diagnostics so far that failures become
  untraceable, and never disable essential error logging to save space or time.
- Respect existing application-specific reporting policies and settings.

## 9. Local storage and temporary files

Covers application databases, settings, caches, autosave and recovery files, temporary
files, session data, local logs, download metadata and converted outputs.

- Store per-user data in the platform's per-user data/config/cache locations, never next
  to the executable or in shared or world-writable locations; restrict permissions to the
  user where the platform allows.
- Create temporary files with secure APIs (unique names, restrictive permissions) in the
  proper temporary location; clean them up on success, failure and cancellation; avoid
  leaving decrypted or converted copies of sensitive files behind.
- Avoid unnecessary copies of sensitive data (caches, thumbnails, debug dumps, duplicate
  exports).
- Never delete legitimate user data to reduce storage; preserve autosave, recovery and
  crash-safety mechanisms and atomic-write behaviour when changing storage code.

## 10. Encryption and credentials

- Distinguish encryption in transit, at rest, file-level, database-level (e.g. SQLCipher),
  credential storage and key management; state which one a feature provides.
- Use established, maintained cryptographic libraries and standard constructions; never
  implement custom algorithms or modes.
- Credentials and keys never live in source code or ordinary configuration files; use
  the operating system's credential store (Windows Credential Manager, macOS Keychain,
  Secret Service/libsecret on Linux) where available ([SECURITY](SECURITY.md)).
- Never claim "encrypted", "secure" or a specific strength without verifying the actual
  implementation, key handling and defaults.
- Never weaken encryption, key derivation or authentication to improve performance or
  convenience (Mandatory safeguard).

## 11. Retention, export and deletion

- Define how long temporary files, caches, logs, diagnostic records, recovery files and
  exported data are kept, and remove them when no longer needed.
- Deletion features delete exactly what they promise: scope deletions narrowly, confirm
  destructive actions, and never touch unrelated user files.
- Do not promise complete or irreversible deletion when backups, recovery files,
  filesystem/SSD remanence, cloud copies or third-party retention may remain; describe
  supported behaviour precisely.
- Export features produce complete, documented, standard formats without including
  unrelated data.

## 12. Scientific and research data

- Treat confidential experimental information, unpublished results, human-participant
  and patient-derived data, medical records, identifiable metadata (sample IDs linked to
  people, dates, locations, image metadata) and restricted datasets as Confidential or
  Highly sensitive.
- Locally stored datasets are not public merely because they are local or downloaded;
  check licences and data-use terms before copying, sharing or publishing them.
- Never expose research data through example fixtures, public repositories, screenshots,
  crash reports or verbose logs.
- Reproducibility never justifies unauthorized disclosure: share code, parameters and
  synthetic or approved data, and reference controlled-access data by accession or
  request procedure ([REPRODUCIBILITY](REPRODUCIBILITY.md)).
- Follow institutional and data-use requirements stated by the maintainer or the project; do
  not invent them.

## 13. Cross-platform considerations

Check per platform where the project supports several: user-data and temporary
directories, file permissions and ACLs, credential stores, log and crash-dump locations
(including OS-level crash reporters), backup and sync behaviour (cloud-synced folders,
system backups), and platform security controls (sandboxing, permission prompts). Do not
assume behaviour verified on one operating system holds on another; report which
platforms were checked.

## 14. Verification

Use evidence proportional to the change. Where relevant:

- inspect network code and dependency capabilities; trace data-collection paths;
- review storage locations, temporary-file handling and cleanup;
- review diagnostic payloads and logs produced by real runs;
- check telemetry and update configuration and their defaults;
- verify credential storage and encryption settings;
- run core features with the network disabled when offline claims are made;
- list third-party services and what they receive;
- inspect release artifacts for personal data, real datasets, personal paths and debug
  dumps.

Report **source inspection** (code read), **runtime verification** (observed behaviour,
e.g. network monitoring or offline run) and **unverified** items separately. Never claim
complete privacy verification, "no data collected" or "fully offline" unless the
necessary checks were actually performed.

## 15. Agent conduct

- Do not upload application source, logs, datasets or user files to external services
  unless the maintainer explicitly asks for that specific transfer.
- Treat user files conservatively: read-only unless the task requires writing; never
  delete or overwrite without confirmation; preserve metadata where expected; back up
  before migrations.
- Keep personal and research data out of commit messages, issue text, CI logs and agent
  reports; quote only what is needed.
