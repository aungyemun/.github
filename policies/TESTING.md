# Testing and Change Verification

Engineering policy for test strategy, execution, automated tests, regression prevention,
cross-platform testing and test evidence. Choosing and maintaining test materials is in
[TEST_DATA_AND_FIXTURES](TEST_DATA_AND_FIXTURES.md); investigating failures, logging and
diagnostics in [TROUBLESHOOTING](TROUBLESHOOTING.md); writing test documentation in
[TESTING_POLICY](documentation/TESTING_POLICY.md).

## 1. Risk-based strategy

Choose test levels from what the application actually does and what could go wrong;
prioritise correctness, reliability, user-data protection and meaningful regression
prevention. Use only the levels that apply:

| Level | Typical use |
| --- | --- |
| Unit / component | pure logic, parsers, calculations, single classes or modules |
| Integration | databases, files, external executables, network services, plugins |
| End-to-end / functional | complete user workflows through the real entry points |
| GUI and visual regression | interaction, layout and rendering ([UI_UX_DESIGN](UI_UX_DESIGN.md) section 12) |
| Security | input validation, path traversal, secrets, permissions ([SECURITY](SECURITY.md)) |
| Performance | baselines and regressions ([PERFORMANCE](PERFORMANCE.md) sections 12-13) |
| Installer and clean-machine | installation, upgrade, uninstall ([INSTALLER_AND_PACKAGING](INSTALLER_AND_PACKAGING.md) section 15) |
| Cross-platform | each supported OS the environment provides |
| Scientific validation | reference outputs with justified tolerances ([SCIENTIFIC_PROVENANCE](SCIENTIFIC_PROVENANCE.md)) |
| Recovery | interruption, crash, disk-full, corrupt input, cancellation |

Do not demand irrelevant categories (no installer tests for a library, no GUI tests for a
CLI).

## 2. Task-aware validation

1. Run the **targeted** tests for the changed code first.
2. Then the relevant **regression** suite for affected components.
3. Then, when the change warrants it: integration, end-to-end, build, packaging or
   application-level checks (release builds: [RELEASES](RELEASES.md)).

Avoid redundant full-suite runs for trivial edits, but never skip tests that cover the
changed behaviour.

## 3. Writing tests

- Add or update tests for fixed bugs (reproduce first, then fix) and for new behaviour,
  including edge cases and error paths.
- Tests are deterministic, isolated (temporary directories, no real user data, no
  network unless explicitly an integration test), fast, and independent of execution
  order, locale, time zone and machine paths. Include Unicode and spaces in paths where
  file handling is involved.
- Tests are maintainable and clearly named after the behaviour they check; they are
  platform-aware (skip with a stated reason, never silently pass, where a platform
  feature is unavailable); setup and cleanup touch only what the test created - never
  unrelated user files.
- Every test asserts the expected result. A process exiting with code 0, a file merely
  existing or "no exception" is not proof that the functionality works; check contents,
  values, state and messages. Failure messages say what was expected and what happened.
- Do not modify a test merely to make an implementation pass. Change a test only if it is
  demonstrably wrong or the intended behaviour legitimately changed - and explain why.
- Never delete, skip, xfail or weaken tests or assertions to get a green run, and never
  disable a test to hide an unresolved failure.

## 4. Templates

Reusable test patterns may be adapted per project - for example unit tests, GUI
interaction tests, document conversion, spreadsheet validation, image processing,
scientific pipeline validation, database migrations, download integrity, installer
upgrades and performance benchmarks. Adapt them to the project's framework and
conventions; never add templates a project does not need.

## 5. Bug-fix regression tests

For a verified defect, when feasible: reproduce the original failure in a test, verify
the correction, exercise relevant edge cases, check nearby functionality, and keep the
existing behaviour. Investigation steps are in [TROUBLESHOOTING](TROUBLESHOOTING.md).

## 6. UI, installers and runtime behaviour

What to check after UI work (functional, multilingual, cross-platform, visual,
regression) is listed in [UI_UX_DESIGN](UI_UX_DESIGN.md) section 12; application icons
in [LOGO_AND_BRANDING](LOGO_AND_BRANDING.md) section 10.
Installer changes are tested against the scenario matrix in
[INSTALLER_AND_PACKAGING](INSTALLER_AND_PACKAGING.md) section 15, in clean virtual
machines or equivalent isolated environments, never against real installations.
Installing dependencies is not runtime verification: smoke-test the affected application
component as described in [DEPENDENCIES](DEPENDENCIES.md) section 11.
For UI changes, verify the real behaviour in the running application when the environment
allows (launch, exercise the changed flow, check layout and errors). Unit tests or code
inspection alone do not verify UI behaviour. If the platform is unavailable (e.g. macOS
from Windows), say so.

## 7. Cross-platform testing

Run tests on each supported operating system the environment or CI provides. A CI matrix
counts as evidence only for runs that completed. Name the platforms tested and those not
tested; never infer one platform's result from another's.

## 8. Reporting (mandatory wording discipline)

Report separately:
- executed and passed (name the command/suite and counts),
- executed and failed (with failing test names and error excerpts),
- skipped or unavailable (and why),
- inspected but not executed,
- unverified.

A test is "passed" only if it was actually executed in this session and completed
successfully. Identify environmental blockers (OS, hardware, missing dependency,
permissions, network) and what remains to be tested. Keep test outputs, coverage reports
and generated results out of Git unless a project deliberately tracks them
([REPOSITORY_HYGIENE](REPOSITORY_HYGIENE.md)).
