# Project Profiles

Optional, public guidance for specific public projects or project categories. Profiles
supplement [DEVELOPMENT_POLICY.md](../DEVELOPMENT_POLICY.md); they never relax security,
privacy or authorization rules.

Register each profile in [index.json](index.json):

```json
{"id": "example-tool", "file": "example-tool.md", "match": {"repos": ["owner/example-tool"]}}
```

Matching keys: `repos`, `application_types`, `distribution_formats`, `platforms`,
`build_systems`, `languages`, `scientific`. A repository's optional gateway may name a
profile with `project_id`. Projects without a profile use the general policy unchanged.
Profiles must contain only information appropriate for public disclosure.
