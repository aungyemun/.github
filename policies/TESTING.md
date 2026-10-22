# Testing and Change Verification

- Run targeted tests for the change first, then the relevant regression suite, then
  integration, build or application-level checks when the change warrants it. Avoid
  redundant runs, but never skip tests that cover the changed behaviour.
- Add tests for fixed bugs and new behaviour, including edge cases and error paths.
- Tests are deterministic and isolated: temporary directories, no real user data, no
  network unless they are explicitly integration tests, no dependence on locale, time
  zone or machine paths.
- Change a test only if it is wrong or the intended behaviour legitimately changed, and
  say why. Never delete, skip or weaken tests to obtain a passing run.
- For UI changes, verify the running application where the environment allows.
- Report separately: executed and passed, executed and failed, skipped (with reasons),
  inspected but not executed, and unverified. "Passed" means executed and completed
  successfully.
