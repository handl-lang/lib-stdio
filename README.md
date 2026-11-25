# lib-stdio

The standard-stream I/O contract — the port side of the port/impl
split, sibling to `lib-posix` (which keeps the raw fd/syscall surface
clean). `src/index.hnl` declares signatures + docs for buffered
stdin/stdout/stderr: `print`/`eprint` (String + newline),
`write`/`ewrite` (raw String-or-Bytes), `flush`, `read_line`
(String-or-null), `read_all` (Bytes to EOF) and `read_chars(n)`.

Buffering follows the C stdio model — stdout line-buffered on a
terminal / block-buffered otherwise, stderr unbuffered, stdin
read-ahead; `flush` forces pending output. Backends (`stdio-c` over
libc via `ffi.call`) export arrows of these types; failures raise
`stdio: <op>: <strerror>`.

## Layout

```text
src/             the .hnl sources
pkg.hnl          package manifest
scripts/hnl-gate.sh      the gate
workflows/gate.yaml            bupha entry (handl-gates)
```

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.
