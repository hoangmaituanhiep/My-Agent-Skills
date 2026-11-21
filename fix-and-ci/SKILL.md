---
name: fix-and-ci
description: Apply after fixing bugs to test through CI before announcing completation. Trigger when user say "Run CI", "push and test pipeline", "test", or after agent finished fixing bugs.
---

# Run CI/CD after fixing bugs

## When to use
After fixing bugs in code.

## Steps
1. **Check the changed files**
  Run 'git status' or 'git diff'. Only files related to the bug can be changed. If there are unrelated files, ask user before continue.

2. **Run local test first**
  Lint, then test. Find commands in 'package.json', 'Makefile', or README.
  If failure happens, continue fixing. Do not stop before passing all local tests.

3. **Add tests that simulating bugs** (If there is no test cover the bug)
  These kind of tests must be failed on old code and passed on new code.

4. **Commit and push on the agent's branch**
  - Do not push on 'main'/'master' branch.
  - Branch's name: 'fix'
  - Commit message: 'fix: <fixing-description>'.

5. **Monitor CI**
  Use 'gh run list --branch <branch> --limit 1' then 'gh run watch <run-id>'.
  - If passed, continue to step 6.
  - If failed, read log with 'gh run view <run-id> --log-failed', fix, come back to step 2.
    Do it max 3 loops, if the process exceeded 3 loops, stop and announce user.

6. **CD**
  - Only automatically deploy on staging environment if pipeline is configured.
  - DO NOT deploy production. Stop and ask for user's permission.

## Report results
Brief: causes of bugs, files changed, tests added, CI status (with run link), and the rest is for user to decide.