<!-- markdownlint-disable -->

# Hardening Report: zkoppert--innersource-crawler/v1.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zkoppert--innersource-crawler/v1.0.4** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files and action.yml use mutable tag references instead of pinned full-length SHA commit hashes or SHA digests. This exposes the action to supply-chain attacks if the referenced tag is moved or the image is replaced.

Failing references:
- .github/workflows/codeql-analysis.yml: `actions/checkout@v4`, `github/codeql-action/init@v2`, `github/codeql-action/autobuild@v2`, `github/codeql-action/analyze@v2`
- .github/workflows/docker-image.yml: `actions/checkout@v4`
- .github/workflows/major-version-updater.yml: `actions/checkout@v4`
- .github/workflows/python-package.yml: `actions/checkout@v4`, `actions/setup-python@v4`
- .github/workflows/release-drafter.yml: `release-drafter/release-drafter@v5`
- action.yml: `image: 'docker://ghcr.io/zkoppert/innersource-crawler:v1'` — mutable image tag instead of a SHA digest

Locations:

- `.github/workflows/codeql-analysis.yml:31`
- `.github/workflows/codeql-analysis.yml:36`
- `.github/workflows/codeql-analysis.yml:43`
- `.github/workflows/codeql-analysis.yml:52`
- `.github/workflows/docker-image.yml:13`
- `.github/workflows/major-version-updater.yml:13`
- `.github/workflows/python-package.yml:18`
- `.github/workflows/python-package.yml:20`
- `.github/workflows/release-drafter.yml:11`
- `action.yml:7`

### script-injection (severity: high)

Sub-rule (a): The 'force update major tag' step in major-version-updater.yml directly interpolates `${{ steps.version.outputs.major }}` inside a `run:` shell command string. GitHub Actions performs YAML template substitution before the shell ever sees the value, so any special characters in the step output are interpreted by the shell. An attacker who can influence the tag name (e.g. via a crafted release tag) could inject arbitrary shell commands.

Offending lines:
  `git tag v${{ steps.version.outputs.major }}`
  `git push origin refs/tags/v${{ steps.version.outputs.major }} -f`

Fix: move the value into an env var and double-quote it in the shell:
```yaml
env:
  MAJOR: ${{ steps.version.outputs.major }}
run: |
  git tag "v${MAJOR}"
  git push origin "refs/tags/v${MAJOR}" -f
```

Locations:

- `.github/workflows/major-version-updater.yml:27`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` key, and no individual job defines a `permissions:` key either. Without explicit permissions, workflows run with the default token permissions, which may be overly broad (e.g. write access to contents, packages, etc.). Each workflow should declare the minimal permissions required.

Affected files:
- .github/workflows/codeql-analysis.yml
- .github/workflows/docker-image.yml
- .github/workflows/major-version-updater.yml
- .github/workflows/python-package.yml
- .github/workflows/release-drafter.yml

Locations:

- `.github/workflows/codeql-analysis.yml:1`
- `.github/workflows/docker-image.yml:1`
- `.github/workflows/major-version-updater.yml:1`
- `.github/workflows/python-package.yml:1`
- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings:

1. **unpinned-uses**: Pinned all action references to full commit SHAs with tag comments for readability. Pinned the docker image in action.yml to its SHA digest while preserving the docker:// scheme and :v1 tag inline.

2. **script-injection**: In major-version-updater.yml, moved `${{ steps.version.outputs.major }}` out of the `run:` shell string into an `env:` block as `MAJOR`, then referenced it as `"v${MAJOR}"` in the shell script.

3. **missing-permissions**: Added minimal `permissions:` blocks at the top level of all 5 workflow files: codeql-analysis.yml (actions:read, contents:read, security-events:write), docker-image.yml (contents:read), major-version-updater.yml (contents:write for tag pushing), python-package.yml (contents:read), release-drafter.yml (contents:write, pull-requests:read).

