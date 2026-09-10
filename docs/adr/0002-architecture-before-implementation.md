# ADR 0002 — Review Target Architecture before implementation

Status: **Accepted constraint**

Date: 2026-09-10

Decision source: Explicit current-task boundary in the supplied checkpoint.

## Context

The existing lab has active services, DNS dependencies and legacy networking. Candidate enterprise VLANs and AD/DNS architecture are unapproved.

## Decision

The current deliverable is a repository and documentation skeleton. Complete and review the Target Architecture as milestone M1. Do not build DC01, create VLAN60, change DNS, clean legacy bridges/templates/alternate VLAN40 or implement automation now.

Apply the engineering sequence per component: manual configuration → documentation → troubleshooting/failure scenarios → automation → reproducibility. Scope the future manual pilot only after M1 acceptance.

## Alternatives

Immediate deployment or immediate automation is outside authorized scope. Treating candidate VLANs as final would bypass required architecture review.

## Consequences and validation

Implementation directories contain responsibility documents only. M1 uses target-review.md and accepted ADRs. No deployable resources or scripts should appear in this skeleton. Design acceptance does not execute changes; operational work needs a defined scope and rollback.
