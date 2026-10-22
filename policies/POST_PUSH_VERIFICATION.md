# Post-Push Verification

After a push, verify the actual remote state when access is available
(`git ls-remote`, `gh run list`, `gh api repos/<owner>/<repo>/commits/<sha>/check-runs`,
`gh release list`):

1. The intended repository and branch.
2. The remote branch points to the pushed commit; report unexpected extra commits.
3. CI for that commit: failing -> investigate; pending -> report as pending; none reported
   -> unverified. None of these is "success".
4. No tag or release was created unless authorized.
5. The pushed commit messages contain no AI co-author trailers, session identifiers,
   conversation URLs or "Generated with ..." footers; report any, without rewriting
   published history.
6. Report remaining problems and anything that could not be verified.

When CI fails: read the logs, decide whether the change caused it, fix within the task
scope, re-run the relevant validation, push a normal follow-up commit and verify again.
Never force-push, rewrite history, disable jobs or weaken tests to hide a failure.
