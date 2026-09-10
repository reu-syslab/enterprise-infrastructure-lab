# Contributing and change control

Current contributions are architecture/documentation changes only. Do not introduce DC01, VLAN60, live inventory, provider/resource configuration, playbooks, scripts or executable CI workflows until the relevant review gate and later implementation scope are accepted.

## Proposed repository workflow

Use short topic branches and reviewed pull requests into `main`. This is a workflow convention in this skeleton; no remote or branch protection is configured. Avoid environment-specific identifiers in branch names and commit metadata. License selection and maintainer identity remain open before public release.

For each change:

1. Identify its backlog item and distinguish CONFIRMED CURRENT STATE, PROPOSED TARGET STATE and OPEN DECISION.
2. Update the authoritative document, then related links/backlog/PROJECT status. Do not silently convert a proposal into a fact.
3. Record an ADR for an architecture choice. Proposed ADRs become accepted only when a review decision is recorded.
4. Follow SECURITY.md; attach only synthetic examples or sanitized conclusions.
5. Document verification, expected failure behavior and rollback when proposing operational changes.
6. Request review using the PR template; acceptance applies to a recorded revision, not later edits.

## Evidence standard

The checkpoint is the source for current-state facts. New facts need a recorded method and sanitized result; raw evidence remains external. Distinguish observed results from expected behavior and unexecuted tests. Consult authoritative vendor documentation during the later technical design review and record the exact versions/assumptions then; this skeleton contains no provider compatibility assertions.
