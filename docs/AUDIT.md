# Website audit — Rafiq Prayer — 2026-11-26

## State
**Fix identified; website download URL is stale on the default branch.** Work is isolated on `fix/audit-2026-11-26`.

## Verified evidence
- Repository: `Aqaaab/rafiq-prayer-website`, default branch `main`.
- Latest Pages workflow: [Run #11](https://github.com/Aqaaab/rafiq-prayer-website/actions/runs/39193011065), commit `f5cd03de8fc51279fe65aa442f84207936d58687`, conclusion `success`.
- The Pages workflow validates `index.html`, `privacy.html`, `favicon.svg`, and Arabic RTL document attributes before deployment.
- GitHub Releases API shows latest release tag `rafiq-prayer-v77` with asset `rafiq-prayer.apk` (19,179,111 bytes).
- The current `index.html` on `main` links to the fixed asset path for **v65**, not v77. This is a reproducible stale-link defect despite the latest Pages workflow being green.
- The release download URL for v77 exists in GitHub release metadata. The website workflow does not test that the CTA points to the latest release asset.

## Required correction
Replace the pinned v65 URL with GitHub's stable latest-release asset endpoint:
`https://github.com/Aqaaab/rafiq-prayer-website/releases/latest/download/rafiq-prayer.apk`.

## Remaining checks
- Verify the PR branch Pages validation workflow passes.
- After merge/deploy, perform an HTTP redirect/download check from a normal browser.
- Coordinate with the private Android source repository: do not publish Debug APKs as public release artifacts. The Android audit records the signing/version blocker.
- The site CI currently checks file presence and RTL metadata only; it does not assert that the download CTA uses the latest-release endpoint.

## Decisions
- No release was created, deleted, or replaced.
- No production website branch was modified.
- A focused link correction is proposed in a separate PR.

## Remediation status in PR #1

- The download CTA now points to `/releases/latest/download/rafiq-prayer.apk` rather than the pinned v65 asset.
- Pages validation now runs for pull requests; deployment is isolated to non-PR events with separate permissions.
- CI asserts the stable latest-release URL is present and rejects pinned `rafiq-prayer-vN` links.
- First PR run `45892498298` failed in the original combined validate/deploy job; its job log was unavailable from the connector, so the exact failure cause is not asserted.
- After splitting validation and deployment, PR run [45892613309](https://github.com/Aqaaab/rafiq-prayer-website/actions/runs/45892613309) passed the `validate` job; `deploy` was correctly skipped for the pull request.
- Live Pages deployment and browser download redirect remain unverified until the change is merged. No merge or public release was performed.
