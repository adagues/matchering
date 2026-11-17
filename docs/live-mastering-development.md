# Live mastering development topology

This document prepares independent feature worktrees for a single final PR. No DSP feature is implemented by this preparation commit.

## Baseline and final PR

- Upstream: https://github.com/sergree/matchering, branch `master`.
- Fork: https://github.com/adagues/matchering.
- Integration: `integration/live-mastering`.
- Planned final PR: integration branch -> this fork's `master`. No automatic upstream PR or merge. A later upstream submission requires a separate decision.
- Keep `master` unchanged. Feature branches start from the shared preparation commit, not from each other's unfinished work.

## Feature worktrees

Local root: `/home/ubuntu/src/matchering-worktrees/`.

| Branch | Directory | Scope / owned paths |
|---|---|---|
| `feat/reproducible-baseline` | `baseline` | Dependency isolation, baseline characterization, test infrastructure: `tests/baseline/`, `tools/live/env.py`, dependency lock and CI files. Coordinate shared configuration changes. |
| `feat/live-diagnostics` | `diagnostics` | Input validation, measurements, manifest: `tools/live/diagnostics/`, `tests/live_diagnostics/`. Do not claim access to an internal limiter envelope without an explicit hook. |
| `feat/true-peak-export` | `true-peak` | Static gain finalization first, qualified true-peak measurement, fixed-codec export checks: `tools/live/finalize/`, `tests/live_finalize/`. No promise for arbitrary transcoding. |
| `feat/controlled-matching` | `matching` | Tonal amount, width controls, reference analysis ranges: `tools/live/controls/`, `tests/live_controls/`. Any core hook requires review; no unaligned wet/dry blend. |
| `feat/48khz-qualification` | `48khz` | Rate, timing, FIR/limiter regression qualification: `tests/sample_rates/`, `docs/sample-rate-qualification.md`. Do not remove upstream warning before evidence. |
| `feat/listening-validation` | `listening` | Anonymous gain-compensated comparisons and CLI documentation: `tools/live/listening/`, `tests/live_listening/`, `docs/listening-protocol.md`. No subjective superiority claims without listening. |

These paths are planned ownership boundaries, not implemented files or APIs.

## Dependencies and integration

1. Baseline isolation and contracts first.
2. Diagnostics, finalization and 48 kHz characterization can proceed independently once baseline contracts are agreed.
3. Controlled matching is informed by diagnostics and rate tests; prefer bounded curve-domain controls over audio blending.
4. Listening tooling consumes validated finalized output and manifests.
5. Only the integrator changes shared core files, shared dependency configuration and this coordination document unless ownership is explicitly reassigned.
6. Integrate reviewed commits into `integration/live-mastering`, in dependency order, with `git merge --no-ff origin/feat/<name>` from the integration worktree. Never merge a feature into `master` directly.
7. Before a feature merges, import already-reviewed prerequisites from integration into that feature if necessary; record the dependency. Do not repeatedly cherry-pick the same prerequisite into sibling histories.
8. Run integration tests and secret scanning after every merge. Open exactly one final PR when ready, against the fork's master. Feature PRs are not required.

## Acceptance and safety

- TDD for new code; baseline characterization is observational, not artificially failing tests.
- Explicit original/bypass/upstream definitions, tolerances, tool versions and provenance.
- Preserve default upstream behavior; new controls are opt-in and regression-tested.
- Two recordings for validation; listening choices distinct from ABX audibility tests.
- No audio, commercial reference, model weights, credentials, token, environment file or private Drive identifier in Git.
- Keep all original licensing and copyright notices. Review GPL-3.0-or-later obligations before distribution.
- Worktrees are local, persistent and disposable only after confirming clean status and preserved commits. No implementation agents are launched by this setup.
