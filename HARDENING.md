<!-- markdownlint-disable -->

# Hardening Report: zkoppert--innersource-crawler/v1.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **zkoppert--innersource-crawler/v1.0.4** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag (`v1`) instead of an immutable SHA digest. This means the action could silently pull a different (potentially malicious) image if the tag is moved. The failing reference is: `image: 'docker://ghcr.io/zkoppert/innersource-crawler:v1'`. It should be pinned to a full SHA256 digest, e.g. `image: 'docker://ghcr.io/zkoppert/innersource-crawler@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:7`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag 'ghcr.io/zkoppert/innersource-crawler:v1' to the immutable digest 'ghcr.io/zkoppert/innersource-crawler@sha256:b376b2592d4eac2e107cbebb6070776e98f6eb609d5c5cf20bf1547f99df41ac'. The original tag is preserved as a comment outside the YAML quotes for readability.

