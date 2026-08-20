<!-- markdownlint-disable -->

# Hardening Report: fkirc--skip-duplicate-actions/v5.3.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **fkirc--skip-duplicate-actions/v5.3.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference third-party actions using mutable version tags (@v3) instead of immutable full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if a tag is moved or a repository is compromised. Affected references: check-dist.yml — actions/checkout@v3, actions/setup-node@v3, actions/upload-artifact@v3; test.yml — actions/checkout@v3 (×4). Each should be pinned to a full SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3.

Locations:

- `.github/workflows/check-dist.yml:16`
- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:28`
- `.github/workflows/test.yml:43`
- `.github/workflows/test.yml:57`

### missing-permissions (severity: medium)

Neither .github/workflows/check-dist.yml nor .github/workflows/test.yml declares a top-level `permissions:` block, and no individual job within either file declares job-level permissions. Without explicit permissions, workflows inherit the default repository token permissions (which may be write-all depending on repository settings), violating the principle of least privilege. A minimal permissions block (e.g. `permissions: contents: read`) should be added.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. check-dist.yml:
   - Added top-level `permissions: contents: read` block
   - Pinned actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3
   - Pinned actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 # v3
   - Pinned actions/upload-artifact@v3 → @ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3

2. test.yml:
   - Added top-level `permissions: contents: read` block
   - Pinned all 4 occurrences of actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3

All SHAs were resolved using lookup_action_sha and are preserved with inline # v3 comments for readability.

