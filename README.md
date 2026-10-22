# .github

Central development standards, security policies, and reusable repository templates for this
account's public repositories, written for human contributors and AI coding assistants.

| Start here | Purpose |
| --- | --- |
| [DEVELOPMENT_POLICY.md](DEVELOPMENT_POLICY.md) | Main development policy and topic index |
| [policies/](policies/POLICY_DIRECTORY.md) | Detailed engineering topics, listed in the policy directory |
| [policies/documentation/](policies/documentation/DOCUMENTATION_INDEX.md) | Documentation and professional writing standards |
| [project-profiles/](project-profiles/README.md) | Optional guidance for specific public projects |
| [gateway/](gateway/POLICY.md) | Template of the optional `.ai-policy/` pointer folder |
| [lessons/](lessons/README.md) | Reviewed, generalised lessons learned |

## Scope and independence

- These documents are development guidance only. They are not dependencies of any
  application, are never needed to build, install, test or run one, and must never be
  shipped inside installers, packages or release assets.
- An application repository needs nothing from this repository. If a project wants AI
  assistants to find these standards, it may contain a single optional `.ai-policy/POLICY.md`
  pointer (see [gateway/](gateway/POLICY.md)); it can be deleted without affecting the
  application.
- These standards stand on their own and do not depend on any other repository.

## Note on GitHub default files

GitHub applies certain files from a public `.github` repository (for example
`CONTRIBUTING.md`, `SECURITY.md`, `CODE_OF_CONDUCT.md`, `SUPPORT.md`, `FUNDING.yml` and
issue templates placed at the root, in `.github/` or in `docs/`) as defaults to every
repository of the account that lacks its own. This repository deliberately keeps such
filenames out of those locations so that it does not change how other repositories appear.

## Versioning

The policy version is in [VERSION](VERSION) and changes are recorded in
[CHANGELOG.md](CHANGELOG.md). Major versions indicate incompatible changes to the gateway schema
or to how the policies are applied; new or extended guidance is a minor version.
