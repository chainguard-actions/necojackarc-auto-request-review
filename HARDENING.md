<!-- markdownlint-disable -->

# Hardening Report: necojackarc--auto-request-review/v0.10.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **necojackarc--auto-request-review/v0.10.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Three `uses:` references in .github/workflows/ci.yml are pinned to mutable tags or branch names instead of immutable 40-character SHA digests. This exposes the workflow to supply-chain attacks if the referenced action is compromised or the tag is moved: `actions/checkout@v2` (line 19), `actions/setup-node@v1` (line 21), and `coverallsapp/github-action@master` (line 39). Each should be replaced with a full commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/ci.yml:19`
- `.github/workflows/ci.yml:21`
- `.github/workflows/ci.yml:39`

### missing-permissions (severity: medium)

The workflow file .github/workflows/ci.yml has no top-level `permissions:` key and the single job `test` also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary (e.g. write access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned action references in .github/workflows/ci.yml: actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e, actions/setup-node@v1 → @f1f314fca9dfce2769ece7d933488f076716723e, coverallsapp/github-action@master → @09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d. Added a top-level `permissions: contents: read` block to restrict the GITHUB_TOKEN to the minimum required permissions.

