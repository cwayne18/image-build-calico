# rancher/hardened-calico

## Daily Copa base-image CVE patching PoC

This repository includes `.github/workflows/copa-base-image-patch.yml`, a daily and manually-triggerable PoC workflow that:

- finds the latest Prime tag matching `vX.Y.Z-buildYYYYMMDD` for `rancher/hardened-calico`,
- scans that published image with Trivy for fixable `HIGH,CRITICAL` OS CVEs (`vuln-type: os`, `ignore-unfixed: true`),
- runs `project-copacetic/copa-action` only when fixable OS CVEs are found,
- pushes the patched image to Prime only (no public push), and
- re-scans and summarizes before/after fixable CVE counts.

Patched images are tagged as `vX.Y.Z-build<UTC_YYYYMMDD>-patchN`, where the build date is the patch day (UTC) and `N` increments for tags that already exist for that day/version prefix.

The workflow uses Prime credentials from Vault via `rancher-eio/read-vault-secrets`, with a fallback to GitHub secrets `PRIME_REGISTRY`, `PRIME_REGISTRY_USERNAME`, and `PRIME_REGISTRY_PASSWORD`.

Current PoC limitation: patching is scoped to the published image reference selected by the registry/runner and does not assemble a new multi-arch manifest list.
