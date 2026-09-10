# Architecture backlog

This file is the project-management source for work items until a remote issue tracker is selected. Statuses: Done (documentation), Open, Blocked, Deferred. Responsible roles are proposed responsibilities, not assigned people. All evidence is public-safe; private findings remain external.

| ID | Work item | Status | Responsible role | Dependency | Acceptance |
| --- | --- | --- | --- | --- | --- |
| REPO-00 | Initial sanitized skeleton | Done (documentation) | Repository maintainer | Project handoff + sanitized live discovery | Required paths, responsibilities, PROJECT/ADRs and no executable deployment code |
| ARCH-01 | Close remaining baseline gaps affecting design | Open | Infrastructure owner | REPO-00 | RB5009 L2 details, resolver/consumer relationships, relevant legacy dependencies, management recovery and capacity documented as sanitized facts |
| ARCH-02 | Review enterprise segmentation | Open | Network designer | ARCH-01 | Accept/reject candidate IDs and boundaries; routing/DHCP/trunk plan and segmentation ADR |
| ARCH-03 | Decide AD and DNS coexistence | Open | Identity/DNS designer | ARCH-01, ARCH-02 | Namespace/topology, authority/forwarding, client DNS, time, failure and rollback design; accepted DNS ADR |
| ARCH-04 | Complete firewall/access matrix | Blocked | Network/security reviewer | ARCH-02, ARCH-03, service needs from ARCH-05 | Exact reviewed flows, isolation/management tests, policy/NAT implications and access ADR |
| ARCH-05 | Define service/pilot and operational requirements | Open | Windows/Linux/monitoring designer | ARCH-02, ARCH-03 | Placement/capacity/licensing, first client, GPO, file permissions, Linux identity, monitoring and recovery criteria |
| ARCH-06 | Review automation/rebuild boundaries | Open | Automation/recovery designer | ARCH-01; final ownership follows ARCH-02–05 | Object owners, bootstrap handoff, private state/secrets/runner trust and replacement recovery prerequisites |
| ARCH-07 | Complete M1 architecture review | Blocked | Project owner and reviewers | ARCH-01–06 | target-review.md complete, blocking ADRs accepted against a reviewed revision |
| REPO-01 | Prepare eventual public release | Open | Repository maintainer | Before remote publication | License decision, deliberate public identity, private disclosure route, publication review and host protections |
| DEBT-01 | Review old bridges | Deferred execution | Virtualization owner | Discovery in ARCH-01; later change approval | Consumer map, migration/rollback plan and separately approved cleanup |
| DEBT-02 | Review untagged templates | Deferred execution | Virtualization owner | Discovery in ARCH-01; later change approval | Dependency map and safe replacement plan |
| DEBT-03 | Review alternate VLAN40 configuration | Deferred execution | Network owner | Discovery in ARCH-01; later change approval | Active binding/dependencies verified; separately approved cleanup |
| BUILD-01 | Scope and perform manual pilot | Deferred | Infrastructure owner | ARCH-07 plus bounded implementation authorization | Reviewed change/runbook, baseline/rollback and pilot acceptance evidence |
| OPS-01 | Execute controlled troubleshooting scenarios | Deferred | Operations owner | Appropriate manual pilot component | Diagnosis and recovery evidence, sanitized scenario records |
| AUTO-01 | Implement reusable automation and CI | Deferred | Automation owner | Manual/document/failure evidence and ownership review | Reviewed code, private-input interface, validation, plan/approval controls and verification |
| REPRO-01 | Demonstrate clean rebuild and hardware replacement recovery | Deferred | Recovery owner | AUTO-01 and approved restore exercise | Measured recovery, expected network/service behavior, documented prerequisites and residual manual steps |

## Immediate review order

Resolve baseline gaps first. Review segmentation, then identity/DNS and service needs. Complete access controls and recovery/automation boundaries using those decisions. Consolidate M1 evidence; do not close ARCH-07 while design-affecting placeholders remain.
