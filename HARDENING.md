<!-- markdownlint-disable -->

# Hardening Report: zkoppert--innersource-crawler/v1.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **zkoppert--innersource-crawler/v1.0.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image with a mutable tag instead of a SHA digest. The reference `image: docker://ghcr.io/zkoppert/innersource-crawler:v1` uses the `:v1` tag, which can be silently updated to point to a different (potentially malicious) image at any time. It should be pinned to a specific SHA256 digest, e.g. `image: ghcr.io/zkoppert/innersource-crawler@sha256:<64-hex-char-digest> # v1`.

Locations:

- `action.yml:7`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag `ghcr.io/zkoppert/innersource-crawler:v1` to the immutable digest `ghcr.io/zkoppert/innersource-crawler@sha256:b376b2592d4eac2e107cbebb6070776e98f6eb609d5c5cf20bf1547f99df41ac # v1`. The comment preserves the human-readable tag name outside the YAML quotes.

