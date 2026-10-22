# Dependencies and Licences

- Justify each new dependency (purpose, maintenance, size, transitive dependencies) and
  prefer the standard library or existing dependencies.
- Use official registries; pin or lock versions; record the versions actually used.
- Check licence compatibility with the project's licence and distribution model, and keep
  third-party notices accurate (see
  [THIRD_PARTY_NOTICES_POLICY](documentation/THIRD_PARTY_NOTICES_POLICY.md)).
- Prefer packages without install-time scripts; avoid abandoned packages; check known
  vulnerabilities when tooling is available.
- Document bundled binaries, models, fonts and data with source, version, checksum and
  licence; never bundle unwanted components.
- Upgrade one logical dependency at a time and run the relevant tests, including a
  packaged-build smoke test for distributed applications.
