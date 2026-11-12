# Security policy

`adaptive-goal-graph` is a non-authorizing planning and contract library.

- Graph structure validation and deterministic ranking do not verify evidence claims or establish workflow approval.
- SHA-256 digest matches establish only equality between supplied bytes and a declared digest. They do not authenticate the source or prove claim truth.
- `AuthenticatedEvidenceSourceResolver` implementations must authenticate the remote peer and source-owning service before returning bytes. No production resolver is included.
- Checkpoint verification requires independently administered authority, attestation, persistent anti-replay, digest appraisal, durable transition-gate, and operation-repository implementations. Missing dependencies must remain `UNKNOWN` or rejected.
- Test doubles in unit tests demonstrate policy branches only. They are not a trust root, external authority, or production security evidence.
- Search results and Plan Graph drafts never authorize execution or dispatch.

Report vulnerabilities privately to the repository owner. Do not publish exploit details in public issues.
