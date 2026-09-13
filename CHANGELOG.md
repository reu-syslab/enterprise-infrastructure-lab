# Changelog

Record reviewed changes to the project and design separately from claims about infrastructure deployment.

## [0.2.0] — 2026-09-13

### Added

- Enterprise server segment VLAN60 (`ENT-SERVERS`).
- Enterprise client segment VLAN70 (`ENT-CLIENTS`).
- FortiGate L3 gateway interfaces for both enterprise VLANs.
- MikroTik tagged VLAN transport between the FortiGate and Proxmox trunks.
- Project execution status and full master implementation plan.
- PRE-LAB baseline workflow for FortiGate, MikroTik and Proxmox.

### Validated

- VLAN60 end-to-end path: VM → Proxmox → MikroTik → FortiGate.
- VLAN70 end-to-end path: VM → Proxmox → MikroTik → FortiGate.
- Static addressing and default gateway operation on both enterprise VLANs.
- 0% packet loss during gateway validation tests.
- Existing Proxmox VLAN-aware `vmbr0` requires no additional bridge configuration for VLAN60/70.

### Discovered

- Confirmed MikroTik RB5009 trunk and access-port roles through live discovery.
- Identified a pre-existing MikroTik management control-plane connectivity issue.
- Confirmed that the management issue does not block normal VLAN transit or Enterprise Lab deployment.

### Changed

- Enterprise Lab moved from architecture-only planning into manual implementation.
- VLAN60 and VLAN70 status changed from proposed to implemented and validated.
- Current implementation target moved to DC01 and the first Active Directory forest.

### Status

The Enterprise Lab network foundation is operational and validated. No Active Directory, enterprise DNS, domain clients or infrastructure automation has been deployed yet. The next implementation milestone is DC01.

## [0.1.0] — 2026-09-10

### Added

- Public repository skeleton and file responsibility map.
- Sanitized current-state checkpoint and proposed target design.
- VLAN/IP, firewall and DNS planning documents.
- Project phases, backlog, review gate and ADR workflow.
- Public/private boundary, contribution guidance and publication checks.
- Runbook/failure-scenario templates and future verification responsibilities.
- Documentation-only Terraform, Ansible, PowerShell, network and CI directories.

### Status

Target Architecture is awaiting review. No infrastructure was deployed or modified. No automation or executable CI workflow was implemented.
