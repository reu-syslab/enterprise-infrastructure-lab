# ADR 0001 — Separate public artifacts from private environment data

Status: **Accepted constraint**

Date: 2026-09-10

Decision source: Explicit project security requirement in the supplied checkpoint.

## Context

The project is intended for a public portfolio. Infrastructure inventory, state and operational outputs can expose environment-specific information even without passwords.

## Decision

Commit only reusable artifacts and synthetic examples. Keep all real environment data, identity/address mappings, secrets, inventory, state/plans, exports, backups and raw evidence outside this repository. Use example namespace `corp.example.test` and example networks within `10.250.x.0/24`. Do not publish a mapping back to the environment.

## Alternatives

Rejected by the stated requirement: live inventory in Git, encrypted live data in public Git, or relying only on ignored files inside the checkout. The actual private storage mechanism remains an OPEN DECISION.

## Consequences

Examples cannot be applied directly as a live environment. Future automation needs a reviewed private-input interface. Public evidence must be sanitized, including metadata and history. `.gitignore` is defense in depth, not a security boundary.

## Validation

Review every public diff, archive and metadata against SECURITY.md. Automated checks are future work and will complement human review. This record does not claim a scanner has been configured.
