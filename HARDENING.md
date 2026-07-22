<!-- markdownlint-disable -->

# Hardening Report: fkirc--skip-duplicate-actions/v5.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **fkirc--skip-duplicate-actions/v5.3.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags (@v3) instead of pinned full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: actions/checkout@v3 and actions/setup-node@v3 in check-dist.yml; actions/checkout@v3 in test.yml.

Locations:

- `.github/workflows/check-dist.yml:19`
- `.github/workflows/check-dist.yml:22`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:28`
- `.github/workflows/test.yml:48`
- `.github/workflows/test.yml:62`

### missing-permissions (severity: medium)

Neither .github/workflows/check-dist.yml nor .github/workflows/test.yml defines a top-level `permissions:` key, and no individual job within either file defines its own `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all unpinned action references to full SHA hashes — actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 (5 occurrences across both files) and actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 (1 occurrence in check-dist.yml), with original tags preserved as comments. (2) Added top-level `permissions:` blocks to both files — check-dist.yml gets `contents: read`, test.yml gets `contents: read` and `actions: read` (the latter needed by the skip-duplicate-actions action to query workflow run history).

