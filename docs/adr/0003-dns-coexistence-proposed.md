# ADR 0003 — AD DNS coexistence with existing resolvers

Status: **Proposed — OPEN DECISION**

Date: 2026-09-10

Related work: ARCH-01, ARCH-03

Reviewer and reviewed revision: Pending

## Context

BIND9 and Unbound exist in VLAN30. Actual authority, forwarding and consumer relationships are not yet documented. AD-integrated DNS is future scope; no namespace or topology is approved.

## Options to evaluate

- Preserve existing non-AD authority/recursion roles and introduce reviewed forwarding for the new AD namespace.
- Use AD DNS for domain clients with a reviewed path to existing non-AD authorities/recursion.
- Redesign resolver role separation later if discovery shows overlap or unacceptable dependency risk.

These options may be combined; none is selected. Compare authority correctness, loop risk, failure domains, access controls, migration effort and rollback.

## Decision

Pending discovery and Target Architecture review. No change to existing DNS is approved by this ADR.

## Required evidence

Sanitized current query paths and consumers; namespace/authority table; proposed query routing by client class; DHCP DNS distribution; recursion/update controls; dependency/failure analysis; migration and rollback checks. Use dns-flow.md as the working design.

## Review record

Pending. On acceptance, record the selected design, rejected alternatives, consequences, reviewer role and exact reviewed revision.
