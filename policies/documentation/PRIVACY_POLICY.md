# Privacy Documentation Policy

How to write user-facing privacy documentation (`PRIVACY.md`, privacy sections in a
README, in-app privacy text). **What** the application must do with data is defined by
the engineering policy [PRIVACY](../PRIVACY.md); this policy covers how to describe it.
Common writing rules are in [GENERAL_WRITING_POLICY](GENERAL_WRITING_POLICY.md).

## When it applies

- Only when a privacy document is needed and the work is authorized: the user asks for
  one, the application transmits data externally, processes sensitive information, or an
  existing privacy document is affected by a change.
- Do not create privacy documents for every repository. A simple offline tool may need
  only a short paragraph, or nothing beyond an accurate README statement.

## Establish the facts first

Privacy statements describe the application's **actual** behaviour, established by
examining code, configuration, dependencies and, where feasible, runtime behaviour - not
by assumption or by copying another project's text. Record for yourself which statements
were verified by source inspection, which by runtime observation, and which remain
unverified; unverified statements are not published as facts.

## Recommended structure (use only the sections that apply)

1. Overview - one or two sentences on the application's overall data behaviour.
2. Scope - which application, versions, platforms and components the document covers.
3. Information processed - categories of data and why.
4. Local processing and storage - what is stored, where (per platform), and for how long.
5. Network communication - every kind of connection the application makes, when and why.
6. Optional connected features - what they send and how to enable or disable them.
7. Telemetry and analytics - present or absent; what is sent; consent or opt-out.
8. Diagnostics and error reporting - local logs, crash reports, what a submitted report
   contains, whether submission is automatic.
9. Third-party services - each recipient and what it receives.
10. Data retention - how long each kind of data is kept.
11. Data export and deletion - what users can export or remove, and the limits.
12. User controls - settings, offline use, how to disable features.
13. Security safeguards - only verified measures (e.g. TLS, encryption at rest).
14. Limitations - what the application cannot control (operating-system backups,
    cloud-synced folders, third-party retention).
15. Updates and contact - only where the maintainer has established a contact and an
    update practice.

## Writing rules

- Professional, plain English; short sentences; concrete nouns ("the application sends
  the version number to ...") instead of vague assurances.
- Describe implemented behaviour only; no plans, intentions or promises the code does
  not keep.
- Distinguish **automatic** from **user-initiated** and **optional** transmission.
- Use "fully offline", "offline-capable" and "connected" as defined in
  [PRIVACY](../PRIVACY.md) section 5. Do not claim "fully offline" or "collects no data"
  unless network code, dependencies, update checks, crash reporters and embedded web
  content were checked. "Core features work locally" is not the same as "no network
  communication".
- Do not overstate deletion or security: no "permanently erased", "unhackable" or
  encryption strengths that were not verified.
- Avoid legal compliance claims (GDPR, HIPAA and similar) unless the maintainer has
  established and documented them; this documentation is not legal advice.
- Never invent contact addresses, data-protection officers, response times or legal
  entities.
- Preserve accurate existing information; change only what is wrong, outdated or within
  the authorized task.
- Keep the document consistent with the README, SECURITY documentation, in-app text and
  the application's real settings.
- Application privacy documentation never needs to mention these development policies.

## Verification before finishing

Check every statement against the evidence list above; check that each listed network
destination, setting name and storage path exists in the current version; check
consistency with related documents; report what could not be verified.
