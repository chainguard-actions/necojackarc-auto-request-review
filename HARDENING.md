<!-- markdownlint-disable -->

# Hardening Report: necojackarc--auto-request-review/v0.9.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **necojackarc--auto-request-review/v0.9.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Three `uses:` references in ci.yml use mutable tags or branch names instead of full 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced action is compromised or the tag is moved:
- `actions/checkout@v2` (line 19)
- `actions/setup-node@v1` (line 22)
- `coverallsapp/github-action@master` (line 38)
Each should be pinned to a full commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/ci.yml:19`
- `.github/workflows/ci.yml:22`
- `.github/workflows/ci.yml:38`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/ci.yml` has no top-level `permissions:` key and the single `test` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the default repository permissions (which may include broad write access). A minimal `permissions:` block (e.g. `contents: read`) should be added at the top level or on the job.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all three action references to full commit SHAs: actions/checkout@v2 → ee0669bd1cc54295c223e0bb666b733df41de1c5, actions/setup-node@v1 → f1f314fca9dfce2769ece7d933488f076716723e, coverallsapp/github-action@master → 09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d. Added a top-level `permissions: contents: read` block to restrict the workflow to the minimum required access.

