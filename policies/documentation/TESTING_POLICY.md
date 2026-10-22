# Testing Documentation Policy

How to document tests and test results. How to test is defined in
[TESTING](../TESTING.md), test materials in
[TEST_DATA_AND_FIXTURES](../TEST_DATA_AND_FIXTURES.md), diagnostics in
[TROUBLESHOOTING](../TROUBLESHOOTING.md).

## Document as relevant
- **Prerequisites and environments:** OS, versions, hardware constraints; frameworks and
  how to install test dependencies.
- **Commands** for unit, integration, end-to-end, regression and performance tests.
- **Fixtures:** where test data lives or how it is generated or downloaded, its
  provenance and licence (synthetic or de-identified; never real personal data), and
  which fixtures are intentionally malformed.
- **Expected results:** how correctness is judged (reference outputs, tolerances).
- **Platform coverage:** platforms tested and platforms not tested.
- **Automated results:** what CI runs and where results appear.
- **Manual tests:** steps for checks that are not automated (e.g. UI walkthroughs).
- **UI testing** and **installer testing** procedures where the project has them.
- **Scientific validation:** reference datasets, tolerances and what the validation does
  and does not establish.
- **Diagnostic procedures:** how to collect logs or enable diagnostic output when a test
  fails.
- **Limitations:** known exclusions, skipped tests (with reasons), untested areas.

## Result statements must distinguish
| Term | Meaning |
| --- | --- |
| implemented | the test exists in the repository |
| executed | it was run, in a stated environment and date/version |
| passed / failed | the observed outcome of that execution |
| skipped | deliberately not run, with the reason |
| not performed | no such testing was done |

Never report success without evidence (a run that actually completed), and never present
unexecuted tests as passed. Do not describe CI as passing while checks are pending. Do
not equate unit-test success with scientific or clinical validation.
