# SECURITY.md Policy

How to write an application's `SECURITY.md`. What the application must do is defined in
[SECURITY](../SECURITY.md); this policy covers documenting it accurately. Create or
change a `SECURITY.md` only when it is needed and within the authorized task - not
automatically for every repository.

## Content (only what is actually established)

- **Supported versions** that receive security fixes, if the maintainer has defined them.
- **Reporting a vulnerability privately:** GitHub private vulnerability reporting (only
  if it is enabled for the repository) or a contact the maintainer has provided.
- **Responsible disclosure:** report privately, allow time for a fix, no public exploit
  details before a fix.
- **Security update practice,** if one exists (e.g. fixes released as patch versions;
  how users are informed).
- **Official release sources:** where genuine releases are published.
- **Verifying downloads:** only the mechanisms that really exist - published checksums,
  code signatures, signed tags - and what each does and does not prove ([SECURITY](../SECURITY.md)
  section 8).
- **Known limitations** (e.g. not designed for untrusted input, no sandboxing, plugins run
  with full privileges, installers not code-signed).

## Never

- Invent security contacts, e-mail addresses, PGP keys, bug-bounty programmes or
  response-time guarantees.
- Promise maintenance or support windows the maintainer has not committed to.
- Claim security features that are not implemented, or present intentions ("we follow
  secure coding practices") as protections; describe only protections that exist and
  can be pointed to.
- Publish exploitable details of unfixed vulnerabilities, internal infrastructure, hosts,
  credentials or confidential configuration.

If no private reporting channel exists, say so and recommend that the maintainer enable
one; do not fabricate one. A `SECURITY.md` in a public account-level `.github` repository
becomes the default for all of that account's repositories lacking their own.
