# Architecture Decision Records

An ADR records context, considered options, a decision, consequences and validation. Statuses: Proposed, Accepted, Rejected, Superseded. “Accepted” requires an explicit review record; file creation is not approval. Accepted project constraints can cite the supplied checkpoint, but cannot approve an unselected design.

| ADR | Status | Scope |
| --- | --- | --- |
| [0001](0001-public-private-boundary.md) | Accepted constraint | Public reusable artifacts; protected environment data external |
| [0002](0002-architecture-before-implementation.md) | Accepted constraint | Target Architecture review before implementation |
| [0003](0003-dns-coexistence-proposed.md) | Proposed | AD DNS coexistence with BIND9/Unbound; unresolved |

Copy [template.md](template.md) for subsequent records. Candidate topics: segmentation, identity topology, management recovery, firewall trust boundaries, resource ownership, CI trust/state/secrets and network replacement recovery. Never silently rewrite an accepted decision; supersede it with a linked record and update PROJECT.md.
