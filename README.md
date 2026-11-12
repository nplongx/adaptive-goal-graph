# adaptive-goal-graph

A Rust library for bounded, deterministic **shadow planning** over Adaptive Goal Graphs (AGGs), plus strict graph contracts, unverified model ranking, byte-level evidence integrity, authenticated evidence-resolution interfaces, and external checkpoint-custody contracts.

The crate is deliberately non-authorizing. It ranks candidate paths and validates supplied evidence metadata/bytes; it does not verify claim truth, approve a workflow, create an executable plan, or dispatch work.

## Terminology

See [TERMINOLOGY.md](TERMINOLOGY.md) for shared identifier, lifecycle, evidence, time-unit, and compatibility conventions.

## Capabilities

- Validate bounded typed graphs, stable identities, constraints, and acyclic nominal transitions.
- Run deterministic bounded beam search with explicit expansion/depth limits and unknown-aware ranking.
- Project a reproduced candidate into a review-only Plan Graph draft with execution disabled.
- Validate and score an offline logistic-regression discriminator; Phase 2 and Phase 3 MCTS outputs remain unverified estimates.
- Enforce strict AGG graph schema v1 and Node-compatible canonical graph identity.
- Compare exact caller-supplied source bytes to declared SHA-256 digests and validate graph/snapshot evidence manifests.
- Define authenticated read-only evidence-resolution interfaces and validate exact response/source binding.
- Define external checkpoint authority, attestation, nonce-ledger, digest-verifier, transition-gate, and checkpoint-operation repository contracts.
- Evaluate checkpoint continuity and fail closed when authority, attestation, durable nonce custody, or projection verification is unavailable.

## Install / dependency source

Use this repository as a standalone Cargo dependency:

```toml
[dependencies]
adaptive-goal-graph = { git = "https://github.com/nplongx/adaptive-goal-graph", tag = "v0.2.0" }
```

All required domain and protocol source is included under `vendor/`, with original license files and immutable source revisions recorded beside each copy. The build uses local path dependencies only; it does not fetch sibling packages or require a parent workspace.

Crates.io publication is disabled because the package intentionally resolves these components from local vendored paths. Consume it as a Git dependency.

## Quick start

```rust
use adaptive_goal_graph::{search, AggGraph, SearchOptions};

fn rank_graph(graph: &AggGraph) -> Result<(), Box<dyn std::error::Error>> {
    let result = search(graph, &SearchOptions::default())?;
    for candidate in result.candidates {
        println!("{}: {:?}", candidate.plan_id, candidate.path_node_ids);
        assert!(!candidate.evidence_verified);
        assert!(candidate.human_review_required);
        assert!(!candidate.dispatch_allowed);
    }
    Ok(())
}
```

## Safety semantics

- Candidate scores are estimates based on caller-supplied metrics. Unknown values remain unknown in Phase 1 ranking; model outputs are explicitly `model_estimate_unverified`.
- A digest match proves byte equality to a declared digest only. It does not prove the source is authentic, durable, independent, or truthful.
- The evidence resolver is a trait boundary. The host must authenticate the actual remote peer and source owner before returning data. No production Execution-service adapter is included.
- Checkpoint custody depends on injected external authority, signature/attestation verifiers, persistent nonce ledger, projection digest verifier, durable transition gate, and saga repository. Test doubles do not establish independent authority.
- Missing or incomplete trust dependencies result in `UNKNOWN` or rejection, never implicit `VERIFIED`.
- `execution_authorized`, `dispatch_allowed`, and evidence-verification flags remain false in search/ranking projections.

This crate is a planner and contract library, not a workflow runtime, trust root, security checkpoint service, source resolver deployment, or authorization system.

## Bounds

Default graph/search limits are intentionally conservative and enforced in code: at most 500 nodes, 2,000 edges, depth 100, beam width 100, and 10,000 expansions. Public search options impose their own bounded defaults. Evidence resolution has a per-source byte limit and a separate aggregate snapshot byte budget.

## Validation

CI tests stable Rust and Rust 1.85 with formatting, dependency metadata resolution, unit tests, Clippy, and rustdoc. The package is self-contained, but its vendored path dependencies have not been separately released to a package registry. The unit suite includes Node-generated canonical graph/discriminator goldens, deterministic search, strict schema rejection, evidence manifest coverage and byte binding, authenticated source metadata checks, checkpoint continuity, exact attestation binding, and nonce replay rejection.

Passing local tests does not verify any external authority, production identity provider, provider execution, deployed source resolver, persistent host transition gate, or real checkpoint service. Integrators must provide and independently operate those systems.

## Source lineage

Created from immutable source snapshot `29c24cfa2b6ead5704d26cf2746bc384f2938b80`. Required contract source is vendored locally; no application runtime, store, worker, provider, MCP, or sandbox module is required.

## License

MIT. See [LICENSE](LICENSE).
