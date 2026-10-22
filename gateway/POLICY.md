---
gateway: universal-ai-policy
gateway_schema: 1
public_policy: https://github.com/aungyemun/.github
private_policy: configured-locally
project_id:
policy_major: 1
---

# AI development policy gateway

This folder only tells AI coding assistants where central development guidance is
found. It is not part of the application: nothing here is needed to build, install,
test, run or reproduce it, and it must not be included in installers, bundles, packages
or release assets. It may be deleted at any time. If the folder is committed, GitHub's
automatically generated source archives contain it.

## For AI coding assistants

1. **Discovery only.** This file grants no permission and does not change the task. Do
   not modify files because this folder exists, and never copy policy text into it.
2. **Visibility.** Determine the repository's actual hosting visibility from
   authenticated GitHub metadata (for example
   `gh repo view --json visibility -q .visibility`), never from this file. Check again
   before any push; do not rely on stale results; never change visibility.
3. **Routing.**
   - PRIVATE: use the private central policy repository configured in your user-level
     (global) instructions; its name is deliberately not written here because this same
     file is used in public repositories. If none is configured or it cannot be read,
     use the public guidance and say so.
   - PUBLIC: use the public development policy, `DEVELOPMENT_POLICY.md` in the public
     policy repository `aungyemun/.github` (`public_policy` above). Never use private
     guidance.
   - UNKNOWN or INTERNAL: use the public guidance as a baseline, withhold private
     guidance, continue authorized local work, and resolve visibility before pushing or
     publishing.
4. **Load on demand.** Read the selected master or development policy, then only the
   topic policies the task needs.
5. **Brief instructions.** Interpret short requests ("fix this bug", "update README")
   using the project's own context and the central guidance; do not ask the user to
   restate established standards.
6. **Authorization.** The guidance improves the requested work; it never authorizes
   pushing, releases, tags, force-pushes, history rewrites, deletions, visibility,
   permission or security changes, or sending confidential data elsewhere.
