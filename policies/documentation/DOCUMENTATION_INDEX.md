# Documentation Policy Index

Read this before creating, reviewing or updating documentation, then the
[General Writing Policy](GENERAL_WRITING_POLICY.md) and only the policies that apply.

| Document | Policy | Applies when |
| --- | --- | --- |
| Any documentation | [GENERAL_WRITING_POLICY](GENERAL_WRITING_POLICY.md) | always, for documentation work |
| `README.md`, README images | [README_POLICY](README_POLICY.md) | any README change: version and status, screenshots, badges, callouts, diagrams, links, layout, verification |
| `CONTRIBUTING.md` | [CONTRIBUTING_POLICY](CONTRIBUTING_POLICY.md) | the maintainer wants contributor guidance |
| `LICENSE`, `NOTICE`, licence headers | [LICENSE_POLICY](LICENSE_POLICY.md) | any licence text (legally sensitive) |
| `SECURITY.md` | [SECURITY_POLICY](SECURITY_POLICY.md) | security reporting documentation |
| `CHANGELOG.md` | [CHANGELOG_POLICY](CHANGELOG_POLICY.md) | any changelog change or release preparation |
| `INSTALLATION.md`, `BUILD.md` | [INSTALLATION_AND_BUILD_POLICY](INSTALLATION_AND_BUILD_POLICY.md) | installing or building |
| `TESTING.md` | [TESTING_POLICY](TESTING_POLICY.md) | documenting tests and results |
| `PRIVACY.md` | [PRIVACY_POLICY](PRIVACY_POLICY.md) | application data behaviour |
| `THIRD_PARTY_NOTICES.md` | [THIRD_PARTY_NOTICES_POLICY](THIRD_PARTY_NOTICES_POLICY.md) | distributed third-party components |
| `METHODS.md` | [METHODS_POLICY](METHODS_POLICY.md) | scientific or research software only |
| Architecture and developer guides | [GENERAL_WRITING_POLICY](GENERAL_WRITING_POLICY.md#accuracy) | describe implemented design only; label proposals |

## Review procedure
1. Read the relevant existing documentation; do not assume any file exists.
2. Compare documented behaviour with the implementation (code, configuration, build
   files, command help, tests).
3. Correct genuine inaccuracies within the task; preserve accurate information and the
   repository's own conventions.
4. Do not rewrite unrelated sections or make purely stylistic changes.
5. Check commands, paths and links where possible; review the diff.
6. Report inconsistencies that remain.

Licence changes, removal of legal notices and changes to scientific conclusions require
explicit maintainer authorization. Application documentation never needs to mention
these policies.
