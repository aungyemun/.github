# Troubleshooting, Logging and Diagnostics

Engineering policy for root-cause investigation, proportionate diagnostic escalation,
application logging and tracing, crash and resource diagnostics, live testing, debugging
reports and development-versus-release diagnostics. Tests and regression tests are in
[TESTING](TESTING.md); privacy of diagnostics in [PRIVACY](PRIVACY.md) section 8;
performance measurement in [PERFORMANCE](PERFORMANCE.md).

## 1. Root-cause investigation

1. **Expected vs actual.** Establish the expected behaviour, the actual behaviour and the
   affected functionality.
2. **Reproduce.** Establish exact steps, inputs and environment (OS, versions, locale,
   paths). Capture the full error text and stack trace.
3. **Collect evidence before editing.** Read logs, configuration, application state,
   existing tests, recent changes (`git log -p`, `git bisect` when a regression window is
   known), dependency versions and the affected code paths end to end.
4. **Narrow the failure boundary** to the smallest component, input and step that shows
   the fault.
5. **Form hypotheses and test them** one at a time with the smallest decisive experiment.
   Record what was ruled out. A secondary symptom is not a root cause.
6. **Fix the root cause**, not the symptom, with a focused change - no unrelated rewrites.
   Do not add retries, sleeps, broad exception handlers, disabled checks or special cases
   that hide the defect. If only a mitigation is possible, label it as such and record
   the root-cause follow-up.
7. **Add a regression test** that fails before the fix and passes after
   ([TESTING](TESTING.md) section 5).
8. **Check related failure paths** (same pattern elsewhere, other platforms, other entry
   points) without expanding scope into unrelated refactoring.
9. **Validate**: targeted tests, then regression/integration/build/app-level checks as
   appropriate.
10. **Report** (section 9).

Common traps: platform path/encoding differences, stale build artefacts or caches,
mismatched dependency versions between dev and packaged builds, frozen-executable
resource paths, locale-dependent parsing, race conditions masked by debugging, and
assuming that an earlier summary of a fix is accurate.

## 2. Proportionate escalation

Start at the lowest level that can produce evidence; escalate only when it does not.

| Level | Situation | Tools |
| --- | --- | --- |
| 1 | simple, reproducible defect | error message, focused reproduction, relevant unit test, targeted logging |
| 2 | intermittent defect | structured logs, state checks, timing, operation IDs, repeated controlled runs |
| 3 | failure across components | stage checkpoints, per-component instrumentation, cross-component tracing, dependency and input/output validation |
| 4 | advanced runtime failure (leaks, hangs, races, performance collapse) | correlated traces, resource monitoring, targeted profiling, OS diagnostics, controlled stress tests |
| 5 | rare or critical failure | isolated reproduction, crash-dump analysis, fault injection, diagnostic builds, state reconstruction - where justified and safe |

Never escalate every bug to heavy instrumentation, and never run destructive tests on
real user data.

## 3. Hard-to-reproduce failures

Collect the available evidence; record an environment profile; compare working and
failing scenarios; narrow the workflow; add focused instrumentation; correlate events by
operation ID; find the earliest verified divergence; test competing hypotheses; fix only
when the evidence supports it; verify with regression tests. State the confidence: do not
claim certainty about a root cause the evidence does not establish.

## 4. Application logging

- Use the application's existing logging framework; never add a second, competing one.
  Configure it in one place.
- Levels as the framework supports them: ERROR, WARNING, INFO, DEBUG, and TRACE where
  available - do not force unsupported levels.
- **Instrument where it helps diagnosis**: startup, configuration, dependency
  initialisation, file operations, data validation, database transactions, network
  communication, external executable calls (command, version, exit code, duration - not
  secrets), background jobs, scientific workflow stages, conversion and export, installer
  operations, error recovery. Do not log every function call.
- **Structure** events where useful, with stable, searchable field names: timestamp,
  severity, component, operation ID, stage, status, duration, error category, exit code,
  software version. Prefer concise events to verbose free text.
- **GUI applications**: diagnose startup failures, missing UI resources, broken commands,
  background-thread exceptions, rendering failures, resize problems, unresponsive
  operations, file-dialog failures and save/export errors. Never log raw keystrokes,
  document contents or unrelated user activity.

## 5. Workflow tracing

For multi-stage work (an analysis, an import, an export), give each run an operation ID
and record start, end, checkpoints, relevant configuration, tool invocations, warnings,
failures, recovery actions and output validation, so one run can be followed through its
components. Do not introduce distributed-tracing infrastructure into a simple local
application without a real need.

**Scientific workflows** keep diagnostic events separate from scientific provenance,
methodology records, quality-control results and reproducibility metadata. Provenance is
retained as long as the results need it, independently of debug-log retention, and traces
never expose confidential input contents
([SCIENTIFIC_PROVENANCE](SCIENTIFIC_PROVENANCE.md) section 6).

## 6. Log privacy, security, retention and overhead

- **Privacy and security:** no passwords, tokens, keys, personal information,
  confidential research data, document contents or sensitive database values; avoid full
  file paths where a file name or component is enough; redact and minimise; never send
  logs externally on your own ([PRIVACY](PRIVACY.md) section 8, [SECURITY](SECURITY.md)
  section 10).
- **Retention:** a per-user log directory with restricted permissions, size limits and
  rotation, a defined retention period and cleanup, and graceful behaviour when the disk
  is full - never unbounded growth. Rotating diagnostic logs never deletes scientific
  provenance records.
- **Overhead:** logging must not cause UI lag, CPU load, excessive disk writes, memory
  growth, startup delay or network traffic. Avoid logging in hot loops, guard expensive
  message construction, use bounded buffers or asynchronous handlers where justified, and
  measure the overhead when it may matter ([PERFORMANCE](PERFORMANCE.md) section 12).

## 7. Crash, resource and live diagnostics

- **Tools:** crash handlers and stack traces, memory and CPU profilers, file I/O tracing,
  database diagnostics (query plans, lock information), network tracing and the operating
  system's crash facilities (e.g. Windows Error Reporting dumps, Linux core dumps or
  systemd-coredump, macOS crash reports). Prefer established tools and temporary use; do
  not add large profiling dependencies permanently when a temporary tool suffices.
  Crash dumps may contain user data: keep them local and protected and delete them when
  the investigation ends.
- **Live application testing:** use safe environments and disposable test data, keep
  user settings intact, avoid destructive operations and unauthorized uploads, record the
  results, and stop at once if a test puts user data at risk. Use the least intrusive
  method that yields the evidence.
- **Cleanup:** remove temporary debug statements, trace captures and instrumentation
  added for the investigation before finishing, unless they are deliberately kept as
  permanent, documented diagnostics.

## 8. Development versus release diagnostics

- **Development builds** may enable DEBUG and targeted TRACE logging, instrumentation,
  profiling, diagnostic checkpoints and extended failure reports.
- **Release builds** generally keep important errors, significant warnings, limited
  operational information where justified, and privacy-conscious crash handling; verbose
  diagnostics are off by default unless the application's requirements say otherwise.
  Remove temporary debugging code. Never remove useful production error reporting merely
  to make the application smaller.
- **Optional diagnostic mode** (only where it helps support): off by default, switched on
  explicitly, explains what it collects, stores locally with bounded retention, easy to
  switch off (or ends automatically after the session or a set period), no hidden
  transmission, acceptable overhead. Not every application needs one.
- **Before a clean release**, decide per application: which logs support needs; which
  diagnostics are development-only; whether anything sensitive can be exposed; the
  performance cost; that rotation is configured; that crash recovery still works; whether
  tracing can be safely re-enabled when needed; where production logs are stored; that
  offline behaviour is unchanged; and that the package contains no development logs,
  trace captures, profiling output, debug datasets, local crash dumps, developer-only
  diagnostics or test-only instrumentation - while keeping the runtime diagnostics and
  error handling the application needs ([RELEASES](RELEASES.md)).
  There is no universal rule to remove all logging.

## 9. Debugging report

Distinguish: observed symptoms; reproduction evidence; diagnostic findings; the confirmed
root cause; plausible but unconfirmed hypotheses; the correction made; tests performed
with results ([TESTING](TESTING.md) section 8); remaining limitations and follow-up.
Preserve useful diagnostics (error messages, failing test names, reproduction steps,
before/after behaviour). Make no claim the evidence does not support.
