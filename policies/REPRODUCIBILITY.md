# Reproducibility

## Independence from development guidance
Anyone must be able to build, install, test and run an application from its documented
dependencies and source code without these policy documents, an `.ai-policy/` pointer
folder, AI tooling, or access to any other repository. Build scripts and packaging
configuration must not reference them, and a checkout with `.ai-policy/` removed must still
build. Application documentation does not need to mention any of this.

## Application reproducibility
- Pin or lock dependencies; record interpreter, compiler and OS versions used for builds.
- Avoid network access during builds beyond declared dependency fetching; avoid embedding
  absolute paths or timestamps where the toolchain allows.
- Scientific software: version inputs and parameters, record random seeds and reference
  data versions, keep environment specifications with the code, and keep results
  traceable to code versions.
- State explicitly when a change alters numerical results, with the reason and magnitude.
