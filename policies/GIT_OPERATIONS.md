# Git Operations

## Authorization
Staging intended changes, committing, pushing to working branches, opening pull requests
and fixing defects within an approved task are normal work. The following each need
explicit authorization for the specific action: pushing when no push was requested,
force-pushing (including `--force-with-lease` and `+refspec`), rewriting published
history, creating or pushing tags, publishing releases, merging protected branches,
deleting remote branches or repositories, and changing visibility, permissions, branch
protection, secrets or security settings.

## Commits
- Stage explicitly and review `git diff --cached` before committing.
- One logical change per commit, with a message that explains what and why.
- Exclude build outputs, caches, environments, IDE files, logs, large binaries and
  personal data ([REPOSITORY_HYGIENE](REPOSITORY_HYGIENE.md)).

## Commit messages
These rules apply to every commit in every repository, whatever the tool used.

- Write a clean, professional message describing the change: a concise subject line
  (imperative mood, about 72 characters or fewer) and an optional body explaining what and
  why. Follow the repository's convention (for example Conventional Commits) if it has one.
  Examples: `Fix PDF text rendering and font fallback`,
  `Improve RNA-seq download integrity validation`.
- Do not add AI attribution or agent metadata, even when a tool adds it by default:
  co-author trailers naming an AI tool (Claude, Codex, ChatGPT, Cursor, Copilot, Gemini,
  Antigravity or any other AI tool), AI session identifiers or conversation URLs, "Generated with/by", "Written with", "Created with", "Assisted by" or
  "AI-generated" footers, `Made-with:`/`Generated-By:`/`AI-*` trailers, model or provider
  names used as attribution, promotional text, invisible characters or substitute
  trailers carrying the same information. Naming an AI technology is fine when it is the
  subject of the change.
- Keep the configured Git author and committer identity. Do not impersonate anyone or
  forge signatures, dates or other metadata. Keep legitimate human co-author,
  `Signed-off-by` and similar trailers.
- Include any trailer or disclosure that a project, organisation or licence explicitly
  requires.
- Where a tool offers a setting to switch automatic attribution off, use it.
- Before pushing, review the messages of all unpublished commits; reword unpublished
  commits only with the maintainer's agreement. Never rewrite published commits
  automatically; report the problem and keep future commits clean.

## Pushing
- Confirm repository, branch and remote; review every outgoing commit
  (`git log -p @{u}..HEAD`) for secrets, personal data, artifacts and AI attribution in the
  commit messages.
- Pushes are fast-forward; if the remote moved, fetch and integrate instead of forcing.
- Never bypass branch protection, required reviews or checks, and never use
  `--no-verify` to skip hooks the maintainer enabled.
- On failures, report the exact error; do not change remotes, protocols or credentials
  unprompted.
