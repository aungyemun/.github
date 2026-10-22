# Development, Architecture and Troubleshooting

## Changes
- Understand the code paths involved before editing; preserve uncommitted work.
- Follow existing conventions; do not reformat unrelated code.
- Smallest change that fully solves the problem; keep settings, saved data, file formats,
  APIs and CLI options backward compatible, or provide a tested migration with backup.
- No hard-coded environment-specific values (user paths, drive letters, machine names);
  handle Unicode and spaces in paths; use platform APIs.
- Explicit error handling: no silently swallowed exceptions or misleading success messages.
- Never silently remove functionality, tests, validation or security checks.

## Architecture
- Extend the established architecture; introduce new structure only with a concrete need.
- Separate UI, domain logic, persistence and platform specifics so logic is testable.
- Validate data at trust boundaries; keep data integrity first (atomic writes,
  confirmation for destructive actions, versioned formats with migrations).
- Offline and local by default; no telemetry or background network traffic without a
  documented opt-in.
- Keep long-running work off UI threads, with progress and cancellation.
- Avoid speculative abstractions.

## Troubleshooting
1. Reproduce and capture the exact error, inputs and environment.
2. Gather evidence (logs, configuration, recent changes, dependency versions) before
   editing.
3. Test one hypothesis at a time.
4. Fix the root cause rather than masking symptoms (no blanket retries, sleeps or broad
   exception handlers); label mitigations as such.
5. Add a regression test that fails before and passes after the fix.
6. Check closely related failure paths without expanding scope.
7. Report root cause, change, tests run and results, and remaining limitations.

## Performance
Measure with representative data before optimising; never trade correctness, data
integrity or security for speed; verify packaged builds, not only development runs.

## Reviewing generated code
Treat earlier automated output and summaries as unverified until checked against the code
and by running the relevant tests.
