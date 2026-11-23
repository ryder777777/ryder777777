# Ryder · kernel & operator numerics

Infinities, NaN propagation, saturation and rounding at the tolerance boundary — in ML device
kernels and the operators built on them — plus the tests that pin the behaviour so a refactor
can't quietly undo it.

Contributing to **[tenstorrent](https://github.com/tenstorrent)**'s open-source stack:
`tt-metal` (ttnn operators, tt-llk / SFPU kernels), `ttnn-visualizer`, `tt-cli`,
`tt-model-manager`, `tt-smi`, `tt-topology`.

India · IST (UTC+5:30) · sahilridder@gmail.com

---

## Merged upstream

| Repo | What landed |
|---|---|
| [`ttnn-visualizer` #2186–#2191](https://github.com/tenstorrent/ttnn-visualizer/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) | 7 PRs — per-op L1 fullness and fragmentation views, `tt-perf-report` error handling, device-log guards |
| [`turso`](https://github.com/tursodatabase/turso/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) | 3 PRs |
| [`unsloth`](https://github.com/unslothai/unsloth/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) · [`feast`](https://github.com/feast-dev/feast/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) · [`pnpjs`](https://github.com/pnp/pnpjs/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) · [`baseten-switch`](https://github.com/basetenlabs/baseten-switch/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) | one fix each |

## Reviewed and approved by maintainers (open, not merged)

All of these are still open. Approved means a maintainer signed off on the diff, not that it landed.

- **[`tt-metal` #61909](https://github.com/tenstorrent/tt-metal/pull/61909)** — APPROVED (LLK review).
  `pack_untilize_quasar` iterated one block per column instead of all of them.
- **[`tt-model-manager` #202](https://github.com/tenstorrent/tt-model-manager/pull/202),
  [#203](https://github.com/tenstorrent/tt-model-manager/pull/203)** — approved by the maintainer,
  who ran the suites himself: 1257 and 1259 passed.
- **[`tt-cli` #86](https://github.com/tenstorrent/tt-cli/pull/86)** — approved, open. `ps` / `report`
  require the `org.tenstorrent.tt-model.profile` label instead of guessing.
- **[`tt-smi` #292](https://github.com/tenstorrent/tt-smi/pull/292)** — open, no review yet. Galaxy reset
  waits for and verifies Ethernet link training; missing links raise instead of half-resetting a 6U silently.

## Open work worth reading

Both tt-metal PRs below currently have **changes requested** — listed as work in progress, not as finished.

- **[`tt-metal` #61902](https://github.com/tenstorrent/tt-metal/pull/61902)** — `log_sigmoid_bw` on the
  stable `exp(-|x|)` form; `hardshrink_bw` for `lambd < 0`. Tests sweep ±20/±80 where the gradient
  saturates, plus a tile-level PCC check against torch.
- **[`tt-metal` #61895](https://github.com/tenstorrent/tt-metal/pull/61895)** — `silu_bw` returned
  `NaN` at `±inf` from `0 * inf`. Guarded on the fp32 path only, so bf16 keeps the six-kernel
  sequence.
- **[`ttnn-visualizer` #2379](https://github.com/tenstorrent/ttnn-visualizer/pull/2379)** —
  `/local/upload/npe` with no `files` part raised an unhandled `IndexError` (HTTP 500); now a 400
  with a regression test. 35/35 in that file.
- **[`tt-model-manager` #213](https://github.com/tenstorrent/tt-model-manager/pull/213)** — scoped
  host chips mapped to a contiguous `/dev/tenstorrent/0..N-1` inside the container, because the
  runtime enumerates from 0.

## How I work

- **Failing test first, then the fix, then the before/after.** Every PR above says what fails
  without it: `assert 500 == 400`, `1259 passed`, a ULP or PCC number.
- **I name what I could not verify.** No TT hardware in my own environment means device-only claims
  are flagged as needing a run, not asserted as done.
- **Async by default.** Six review rounds on #61909 happened across weeks and timezones.

## Stack

Python · Go · C++ (kernel-adjacent) · Rust (reading, small changes) · pytest / Flask / pydantic ·
Docker · git at scale (rebases and conflict resolution in a 60k-commit repo) · GitHub Actions and
the REST / GraphQL API.
