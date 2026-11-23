# Ryder

Small, scoped patches to codebases I don't own. One behaviour per pull request, with a test that
fails before the change and passes after. Everything below is merged and public — click through.

## Merged upstream

| Repo | What landed |
|---|---|
| [turso](https://github.com/tursodatabase/turso/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) | 3 fixes — MVCC cursor position after a miss, PRIMARY KEY autoindex order across `ALTER TABLE`, deferring side effects for postgres prepared statements |
| [ttnn-visualizer](https://github.com/tenstorrent/ttnn-visualizer/pulls?q=is%3Apr+author%3Aryder777777+is%3Amerged) | 7 PRs — per-operation L1 fullness and fragmentation views, `tt-perf-report` error handling, device-log read guards |
| [unsloth](https://github.com/unslothai/unsloth/pull/14722) | pass `trust_remote_code` for checkpoint exports |
| [feast](https://github.com/feast-dev/feast/pull/7062) | `watch` verb on the operator ClusterRoles |
| [pnpjs](https://github.com/pnp/pnpjs/pull/3379) | non-string `err.code` in `NodeFetchWithRetry` |
| [baseten-switch](https://github.com/basetenlabs/baseten-switch/pull/98) | `errors.Is(err, os.ErrNotExist)` in config loading |

## How I work

- **The failure comes first.** Each patch above reproduces a bug before it fixes one — a failing
  assertion, an error code, a ULP or PCC number.
- **I say what I could not verify.** If a claim needs hardware or an environment I don't have, I
  write that down instead of asserting it.
- **Async by default.** Review happens in writing, across time zones.

## Contact

India · IST (UTC+5:30) · sahilridder@gmail.com — open to scoped contract work.
