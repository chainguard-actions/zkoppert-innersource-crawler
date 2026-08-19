<!-- markdownlint-disable -->

# Hardening Report: zkoppert--innersource-crawler/v1.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zkoppert--innersource-crawler/v1.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple files reference actions and Docker images by mutable tags/versions instead of full 40-character SHA commit digests, making them vulnerable to supply-chain attacks.

• action.yml: `image: 'docker://ghcr.io/zkoppert/innersource-crawler:v1'` — mutable tag, not a SHA digest.
• codeql-analysis.yml: `actions/checkout@v2`, `github/codeql-action/init@v1`, `github/codeql-action/autobuild@v1`, `github/codeql-action/analyze@v1`.
• docker-image.yml: `actions/checkout@v2`.
• linter.yml: `actions/checkout@v2.3.4`, `uses: docker://ghcr.io/github/super-linter:latest` (mutable `latest` tag).
• python-package.yml: `actions/checkout@v2`, `actions/setup-python@v2`.
• release-drafter.yml: `release-drafter/release-drafter@v5`.

All references should be pinned to a full 40-character hex commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`), and Docker image references should use a SHA digest (e.g. `ghcr.io/zkoppert/innersource-crawler@sha256:<digest>`).

Locations:

- `action.yml:7`
- `.github/workflows/codeql-analysis.yml:31`
- `.github/workflows/codeql-analysis.yml:36`
- `.github/workflows/codeql-analysis.yml:43`
- `.github/workflows/codeql-analysis.yml:55`
- `.github/workflows/docker-image.yml:14`
- `.github/workflows/linter.yml:28`
- `.github/workflows/linter.yml:33`
- `.github/workflows/python-package.yml:18`
- `.github/workflows/python-package.yml:20`
- `.github/workflows/release-drafter.yml:11`

### missing-permissions (severity: medium)

None of the active workflow files define a top-level `permissions:` key, and no individual job within them defines job-level `permissions:` either. Without explicit permissions, workflows run with the default (often overly broad) token permissions. Each workflow should declare the minimal required permissions (e.g. `permissions: read-all` or specific scopes like `contents: read`).

Affected files: codeql-analysis.yml, docker-image.yml, linter.yml, python-package.yml, release-drafter.yml.

Locations:

- `.github/workflows/codeql-analysis.yml:1`
- `.github/workflows/docker-image.yml:1`
- `.github/workflows/linter.yml:1`
- `.github/workflows/python-package.yml:1`
- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action/image references by resolving real commit SHAs and container digests via lookup tools. Pinned: actions/checkout@v2 → SHA 0717577d, actions/checkout@v2.3.4 → SHA 5a4ac900, github/codeql-action/{init,autobuild,analyze}@v1 → SHA 231aa2c8, actions/setup-python@v2 → SHA e9aba2c8, release-drafter/release-drafter@v5 → SHA 09c613e2, ghcr.io/zkoppert/innersource-crawler:v1 → sha256:b376b259 (docker:// scheme preserved), ghcr.io/github/super-linter:latest → sha256:b723c817. Added minimal top-level permissions blocks to all 5 workflow files: codeql-analysis.yml gets contents:read + security-events:write; docker-image.yml and linter.yml and python-package.yml get contents:read; release-drafter.yml gets contents:write + pull-requests:read.

