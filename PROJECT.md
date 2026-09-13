# Project control document

Version: 0.1.0 — 2026-09-10

Current phase: **P1 — Target Architecture definition and review**

Next milestone: **M1 — Target Architecture reviewed and accepted**

Approval status: **Not yet accepted; implementation is blocked by design.**

## Project goal

Build a portfolio-quality, enterprise-style infrastructure lab on Proxmox that demonstrates network isolation, Windows and Linux administration, identity, DNS, permissions, monitoring, disciplined troubleshooting, and eventual rebuilds from reusable code plus protected environment data.

Coverage includes Proxmox VE, FortiGate, MikroTik RouterOS, Windows Server, Active Directory, AD-integrated DNS, Group Policy, Windows 11 clients, Windows File Server, SMB/NTFS permissions, Linux, BIND9/Unbound, Kerberos/SSSD, VLANs/routing/firewall policies, Zabbix, PowerShell, Ansible, Terraform, CI/CD, and failure scenarios.

## Principles

1. Manual configuration → documentation → troubleshooting/failure scenarios → automation → reproducibility.
2. Preserve the current environment while designing changes; discover dependencies before migration or cleanup.
3. Separate observed facts, candidate design, open decisions, and accepted constraints.
4. Keep all real environment data outside public Git, including history, issues, artifacts and logs.
5. Define verification and rollback before a change; record evidence without exposing the environment.
6. Give each managed object one authoritative automation owner; avoid overlapping writes.
7. Preserve an independent administrative recovery path and isolate enterprise identity from WAN-exposed services.
8. Do not imply success from a placeholder, a plan, or a command that has not been executed.

## Phases

| Phase | Deliverable and exit gate | Status |
| --- | --- | --- |
| P0 — Baseline discovery | Sanitized current-state documentation from supplied context plus live read-only discovery; unknowns recorded | Completed enough to enter P1: Proxmox and FortiGate audited live; exact RB5009 L2 configuration remains a recorded non-blocking gap |
| P1 — Target Architecture | Reviewed network, DNS, identity, access, migration and ownership design; decision record | **Current; M1 pending** |
| P2 — Manual pilot | Separately authorized isolated pilot; manual build records, rollback and positive/negative verification | Deferred |
| P3 — Operational understanding | Runbooks and controlled failure scenarios with diagnosis, recovery and sanitized evidence | Deferred |
| P4 — Automation | Approved Terraform/Ansible/PowerShell/vendor ownership; validation, plan and gated private execution | Deferred |
| P5 — Reproducibility | Clean rebuild and same-model replacement recovery demonstrated and documented | Deferred |

The manual/document/failure/automation cycle repeats per component. Later phases do not permit skipping earlier evidence.

## Completed discovery work

**CONFIRMED CURRENT STATE — supplied context plus live read-only discovery**

- Documented Internet → FortiGate 80F → 802.1Q trunk → MikroTik RB5009, with branches to the physical MGMT workstation and the Proxmox trunk.
- Live-audited FortiGate as gateway, DHCP server, inter-VLAN router and firewall, including WAN NAT/VIPs; standalone NAT mode, FortiOS 7.6.2, no system zones. Interface/VLAN roles, routing, DHCP scopes, DNS settings, policies, VIPs, address objects and custom services were reviewed privately and only sanitized conclusions are published.
- Recorded existing VLAN10/20/30/40/50 purposes using synthetic subnets in public documentation.
- Live-audited Proxmox VE 9, VLAN-aware `vmbr0`, tagged VM attachment and VLAN20 management. Current VM/LXC network attachment patterns were reviewed privately and only sanitized conclusions are published.
- Recorded old `vmbr1`/`vmbr2` references and untagged templates as preserved debt.
- Confirmed BIND9 and Unbound in VLAN30. FortiGate itself uses the BIND9 resolver; DHCP on selected existing segments advertises BIND9, while Wi-Fi/DMZ use FortiGate default DNS service. BIND9/Unbound authority, forwarding, recursion and complete consumer relationships remain open.
- Recorded the dedicated VLAN20 Twingate connector for direct Proxmox/API access.
- Recorded WAN-exposed DMZ/test/game services and the requirement to isolate new AD services.
- Recorded the legacy alternate VLAN40 configuration as a cleanup candidate, without approving removal.

Live read-only CLI discovery was performed on Proxmox and FortiGate. No infrastructure change, cleanup, packet capture or deployment was performed. Exact RB5009 RouterOS port/PVID/tagging configuration remains pending live verification.

## Current phase

The documentation skeleton and initial network foundation are complete. VLAN60 ENT-SERVERS and VLAN70 ENT-CLIENTS are implemented and validated end-to-end. The next implementation slice is DC01 and the first Active Directory forest. Infrastructure automation remains deferred until the corresponding components have been built, tested and troubleshot manually.

## Next actions

| Order | Action | Completion evidence |
| --- | --- | --- |
| 1 | Resolve baseline gaps privately: resolver flows, consumers, bridge/template dependencies, management paths | Sanitized findings in current-state document; no raw exports |
| 2 | Record the accepted VLAN60 ENT-SERVERS and VLAN70 ENT-CLIENTS design; review additional segmentation only when required by later slices | VLAN/IP plan and architecture documentation reflect implemented and validated state |
| 3 | Design AD namespace, DC topology, DNS authority, forwarding, DHCP DNS options and migration | DNS flow document and DNS ADR; failure/rollback criteria |
| 4 | Define access by source, destination, purpose and protocol | Reviewed firewall matrix; explicit denied paths and verification cases |
| 5 | Define Windows/Linux pilot, GPO boundaries, SMB/NTFS model, identity and monitoring | Target service/identity decisions, capacity assumptions and acceptance criteria |
| 6 | Select future object ownership, state/secrets model, runner trust and replacement recovery approach | Automation boundary and recovery ADRs; no implementation |
| 7 | Complete M1 review | Completed target-review checklist, decision log and accepted architecture revision |

Track detailed work in [backlog.md](docs/project/backlog.md). Roles below identify responsibilities only; no person or account has been assigned.

## Accepted decisions

| ID | ACCEPTED CONSTRAINT | Record |
| --- | --- | --- |
| A01 | Public reusable artifacts and synthetic examples; all real environment data kept externally | [ADR 0001](docs/adr/0001-public-private-boundary.md) |
| A02 | Architecture review precedes each implementation slice; VLAN60/70 are implemented and validated, DC01 is the next manual slice, and automation remains deferred until manual understanding exists | [ADR 0002](docs/adr/0002-architecture-before-implementation.md) |
| A03 | New AD environment must be isolated from WAN-exposed DMZ/test/game services | Target-state security requirements |
| A04 | Preserve old bridges, templates and alternate VLAN40 configuration until separately reviewed | Current-state debt register |
| A05 | Plan AD DNS before changing BIND9/Unbound or existing clients' DNS | DNS design draft |
| A06 | First domain client must not be the primary physical MGMT workstation | Target pilot requirements |
| A07 | Eventual device rebuilds must use reusable automation and protected data, including equivalent same-model replacement hardware | Recovery design requirements |
| A08 | Eventual delivery uses validation → plan → approval → private/self-hosted execution → verification | Automation design draft |

Accepted constraints do not approve a concrete target topology or firewall rule set.

## Open decisions

**OPEN DECISION** — all items below require review.

| ID | Decision | Tracked work |
| --- | --- | --- |
| O01 | Enterprise segmentation, final VLAN IDs, addressing and routing/DHCP placement | ARCH-02 |
| O02 | AD forest/domain namespace, DC count, service placement, redundancy and time hierarchy | ARCH-03 |
| O03 | AD DNS coexistence with BIND9/Unbound, authority, forwarding, DHCP DNS distribution, failure behavior | ARCH-03 |
| O04 | Exact permitted/denied flows, management access, zone usage and exposure boundary | ARCH-04 |
| O05 | Windows versions/editions/licensing, pilot resource budget, OU/GPO and SMB/NTFS model | ARCH-05 |
| O06 | Linux distribution, Kerberos/SSSD integration scope, authorization and recovery access | ARCH-05 |
| O07 | Zabbix placement, monitored assets, collection direction and alert acceptance | ARCH-05 |
| O08 | Providers, object ownership, MikroTik automation method, state backend, secret source and runner controls | ARCH-06 |
| O09 | Backup scope, RPO/RTO, out-of-band/bootstrap prerequisites and hardware/firmware compatibility | ARCH-06 |
| O10 | Legacy cleanup dependencies, sequence and separately authorized change window | DEBT-01–03; deferred beyond M1 execution |
| O11 | Repository license, public maintainer identity and remote publication settings | REPO-01 |

## Known technical debt

| ID | Debt | Treatment now |
| --- | --- | --- |
| DEBT-01 | Legacy VMs reference `vmbr1`/`vmbr2` | Inventory dependencies; preserve bridges and attachments |
| DEBT-02 | Some old templates have untagged networking | Assess consumers before any template conversion |
| DEBT-03 | Alternate legacy VLAN40 configuration from abandoned home-automation experiment | Clarify active bindings and dependencies; no deletion |

Unknown DNS relationships and resource capacity are discovery gaps, not proven faults. WAN exposure is a confirmed design constraint; this repository does not assert a security incident.

## Security rules

- Never commit real addresses/subnets/domains, public addresses, MACs, serials, account names, passwords, tokens, API keys, VPN endpoints, device exports or other environment-specific data.
- Use only synthetic examples such as `corp.example.test` and `10.250.x.0/24`; maintain real mapping and inventories outside this checkout.
- Keep state, plans, backups, logs, packet captures, credentials and private evidence external; treat these as potentially sensitive even if encrypted.
- Review filenames, diffs, commit metadata, screenshots, archives, issue text and CI output before publication. An ignore file or scanner alone cannot guarantee safety.
- Future untrusted pull requests must never reach private runners, secrets or management networks.
- Do not expose AD services through existing WAN NAT/VIPs. Any necessary enterprise-to-other-segment access needs a reviewed flow and negative isolation tests.
- Follow [SECURITY.md](SECURITY.md) for handling accidental disclosure and publication checks.

## Definition of done

### This skeleton

- [x] Required paths exist and have documented responsibilities.
- [x] Current facts, target proposals and open choices are distinguishable.
- [x] PROJECT, backlog, ADRs, review checklist and operational templates exist.
- [x] Only synthetic environment examples are present.
- [x] No deployable infrastructure, provider configuration, scripts or executable CI workflows are included.

### M1 — Target Architecture

- [ ] All [target-review.md](docs/architecture/target-review.md) blocking checks resolved with evidence.
- [ ] Final segmentation, namespace/DNS, identity, access flows and service placement accepted.
- [ ] Migration, rollback, management recovery and first-client plan accepted.
- [ ] Future automation boundaries, sensitive-data handling and recovery feasibility documented.
- [ ] Accepted ADRs and reviewer/owner decision recorded against the reviewed revision, using public-safe role labels.
- [ ] No unresolved choice that changes pilot safety or topology; explicitly deferred work has a reason, responsible role and gate.

### Eventual project completion

Every in-scope component has a manual build record, sanitized troubleshooting/recovery evidence, reusable automation, independently supplied private environment data, verification and a documented rebuild exercise. Network replacement recovery is demonstrated on equivalent same-model hardware under recorded prerequisites. Completion claims require evidence, not just code.
