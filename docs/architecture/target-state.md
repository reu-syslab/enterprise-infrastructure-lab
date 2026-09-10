# Target Architecture — review draft

**PROPOSED TARGET STATE. Not approved and not deployed.**

This document defines the design questions and required outcomes for M1. The current-state document remains the baseline. No proposed subnet, service placement, object name or automation directory is permission to implement it.

## Accepted boundaries

Preserve current services and administration while designing an isolated enterprise lab. New AD services must be isolated from WAN-exposed DMZ/test/game workloads. The primary MGMT workstation must not be the first domain client. Plan DNS coexistence before modifying BIND9, Unbound or clients. Keep protected environment data external. Do not clean legacy bridges, templates or alternate VLAN40 yet.

## Candidate segmentation

| Candidate | Intended purpose | Status |
| --- | --- | --- |
| VLAN60 | Enterprise Servers | PROPOSED TARGET STATE; ID and scope require review |
| VLAN70 | Windows Clients | PROPOSED TARGET STATE; ID and scope require review |
| VLAN80 | Linux | PROPOSED TARGET STATE; ID and scope require review |
| VLAN90 | Monitoring/Test | PROPOSED TARGET STATE; combined trust boundary requires review |

The candidate reuses FortiGate routing/firewalling and RB5009/Proxmox trunks, subject to review of capacity, reachability and isolation. Gateway/DHCP placement for new segments, allowed VLAN propagation, access-port membership and management exceptions are not finalized. Do not assume Monitoring and Test should share a trust boundary.

## Service and identity design

| Area | Intended outcome | OPEN DECISION / required design evidence |
| --- | --- | --- |
| AD and Windows Server | Domain identity and policy in an isolated lab | Namespace, versions/licensing, DC count/placement, capacity, time hierarchy and recovery; `DC01` is only a future discussion label |
| AD-integrated DNS | Domain-authoritative DNS with reviewed coexistence | Authority, delegation/forwarding, recursion restrictions, dynamic updates, resolver failure behavior and DHCP DNS settings |
| Windows 11 clients | Reversible pilot domain join, later optional physical clients | First isolated virtual/test client, edition/licensing, OU placement, GPO scope and recovery; primary MGMT workstation excluded from first-client role |
| Group Policy | Controlled policy testing and promotion | OU design, linking/filtering, rollback and separation of administrative access |
| Windows File Server | Documented SMB share and NTFS access model | Placement/storage, group model, inheritance, effective permissions, access-denied tests and backup/recovery |
| Linux | Linux services and optional domain-backed login | Distribution/version, Kerberos/SSSD scope, time/DNS dependencies, authorization mapping and fallback administration |
| BIND9 / Unbound | Preserve existing consumers during coexistence | Actual current chain, zone ownership, query routing, loop avoidance and eventual role boundaries |
| Zabbix | Verify availability and expose controlled failures | Placement, protocol/direction, identity/permissions, alert routes and thresholds; no agent/port assumptions yet |
| Proxmox | Predictable guest networking and recoverable VM lifecycle | Capacity, templates, VLAN tagging, backup boundaries and legacy coexistence |

Example namespace `corp.example.test` is documentation-only. Domain/forest topology, hostnames and every actual address remain undecided and external.

## Required access properties

- No WAN publication of AD services through NAT/VIPs.
- No unsolicited access from WAN-exposed workloads into the enterprise identity environment; exact denial scope and any necessary outbound flows require review.
- Explicit, minimal enterprise service flows with named purpose, direction, verification and rollback.
- Independent administrative/recovery access that survives a domain or DNS outage; the existing Twingate connector is confirmed, but sufficiency as the recovery path is unproven.
- Resolver access and management controls documented per trust boundary.

Use [firewall-matrix.md](../network/firewall-matrix.md) as the review register. It contains policy intent, not installable rules.

## Change and failure planning

Before the future manual pilot, define private baseline/backup capture, health checks, approved change window, validation checkpoints and rollback triggers. Account for trunk reachability, VM tag errors, DNS loops/unavailability, domain/time failures and loss of management. Restore current client DNS and network attachment only through a reviewed rollback procedure; no ad hoc cleanup is part of the pilot.

Dependencies to resolve: segmentation → DNS/identity and service placement → access matrix → migration/rollback and verification → M1 review. After M1, scope and authorize the manual pilot separately.

## Eventual automation and recovery

The required direction is Git → CI validation → plan → approval → private/self-hosted runner → FortiGate/MikroTik/Proxmox → Windows/Linux configuration → verification/monitoring. This is a future control flow, not an existing pipeline. See [automation-boundaries.md](automation-boundaries.md) for ownership and trust decisions.

Equivalent same-model network hardware must eventually be recoverable using reviewed reusable automation plus protected environment data/secrets. The design must identify manual bootstrap prerequisites, firmware compatibility, protected backups, secret rehydration, interface mapping, recovery access and restore verification. Recovery feasibility is reviewed at M1; successful rebuild evidence belongs to later phases.
