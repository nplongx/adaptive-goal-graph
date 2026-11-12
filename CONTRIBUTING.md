# Contributing

Keep this crate independent of workflow runtime, database/store, worker, provider, MCP, and sandbox implementations. Its external dependencies must remain the independently published `workflow-domain` and `protocol-contracts` crates, pinned to reviewed immutable revisions.

Run:

```sh
cargo fmt --all -- --check
cargo test --locked
cargo clippy --all-targets --locked -- -D warnings
cargo doc --no-deps --locked
cargo metadata --locked --format-version 1
```

Any change to bounds, canonical graph identity, evidence manifest semantics, custody continuity, or `UNKNOWN`/non-authorization behavior requires focused regression tests and explicit compatibility notes.
