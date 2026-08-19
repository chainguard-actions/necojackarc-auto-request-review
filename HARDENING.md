<!-- markdownlint-disable -->

# Hardening Report: necojackarc--auto-request-review/v0.12.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **necojackarc--auto-request-review/v0.12.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file .github/workflows/ci.yml uses three action references pinned to mutable tags or branch names instead of full 40-character commit SHAs: `actions/checkout@v2` (line 19), `actions/setup-node@v1` (line 28), and `coverallsapp/github-action@master` (line 43). These can be silently updated by the upstream maintainer, enabling supply-chain attacks.

Locations:

- `.github/workflows/ci.yml:19`
- `.github/workflows/ci.yml:28`
- `.github/workflows/ci.yml:43`

### script-injection (severity: high)

Rule (b) violation: The `run:` block at line 21–23 of ci.yml expands the shell variable `$REF` without double-quoting it in both `git fetch origin $REF` and `git checkout $REF`. The variable `REF` is sourced directly from `${{ github.event.pull_request.head.sha || github.sha }}` (an attacker-controllable `github.*` context value via `pull_request_target`). An unquoted expansion allows shell metacharacters in the value to be interpreted by the shell. The fix is to quote the variable: `git fetch origin "$REF"` and `git checkout "$REF"`.

Locations:

- `.github/workflows/ci.yml:21`

### missing-permissions (severity: medium)

The workflow file .github/workflows/ci.yml has no top-level `permissions:` key and the single job `test` also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default repository permissions (which may include broad write access). Explicit minimal permissions should be declared, especially since the workflow uses the `pull_request_target` trigger which runs with write access to the base repository.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings in .github/workflows/ci.yml:
1. unpinned-uses: Pinned actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e, actions/setup-node@v1 → @f1f314fca9dfce2769ece7d933488f076716723e, and coverallsapp/github-action@master → @09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d. Original tags preserved as inline comments.
2. script-injection: Double-quoted $REF in both `git fetch origin "$REF"` and `git checkout "$REF"` to prevent shell metacharacter injection from the attacker-controllable github context value.
3. missing-permissions: Added top-level `permissions: contents: read` block to restrict the workflow to the minimum permissions needed (read-only repository contents for checkout and git operations).

