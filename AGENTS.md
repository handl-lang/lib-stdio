# lib-stdio

The standard-stream I/O **contract** — the port side of the port/impl
split, sibling to `lib-posix` (which keeps the raw fd/syscall surface
clean). `src/index.hnl` declares signatures + docs for buffered
stdin/stdout/stderr. Backend: `stdio-c` over libc via `ffi.call`.

## Layout

| path | role |
| --- | --- |
| `src/index.hnl` | the contract — signatures + docs |
| `pkg.hnl` | manifest — `modules: ["stdio"]` |
| `scripts/hnl-gate.sh`, `workflows/gate.yaml` | the gate / bupha entry |

## Contract

- Surface: `print`/`eprint` (String + newline), `write`/`ewrite`
  (raw String-or-Bytes), `flush`, `read_line` (String-or-null),
  `read_all` (Bytes to EOF), `read_chars(n)`.
- Buffering follows the C stdio model — stdout line-buffered on a
  terminal / block-buffered otherwise, stderr unbuffered, stdin
  read-ahead; `flush` forces pending output.
- Failures raise `stdio: <op>: <strerror>`.
- Pure `.hnl`; no Rust crate.

## Verification

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
