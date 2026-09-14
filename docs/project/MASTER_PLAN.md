# Enterprise Infrastructure Lab — MASTER PLAN

**Project type:** Ephemeral enterprise infrastructure training environment

**Hosting platform:** Existing HomeLab

**Execution model:** Manual first → evidence → troubleshooting → automation → self-healing → rebuild → teardown

**Primary source of current progress:** `docs/project/STATUS.md`

**Private operational handoff:** stored outside the public repository

**Plan status:** Living document

---

# 0. Executive goal

Build a realistic enterprise infrastructure environment on top of the existing HomeLab, operate it manually, document it, deliberately break it, troubleshoot it, automate it, recover it after incidents, rebuild it from code, verify the rebuilt environment, and finally remove it without leaving unexpected drift in the underlying HomeLab.

The project is intentionally larger than a basic Active Directory lab.

It combines:

- enterprise networking
- Windows Server
- Active Directory
- DNS
- Group Policy
- Windows clients
- file services
- Linux integration
- monitoring and observability
- patch management
- backup and restore
- incident response
- automated remediation
- infrastructure as code
- configuration management
- secrets management
- CI/CD
- disaster recovery
- reproducibility
- final teardown

The project is temporary.

The HomeLab is permanent.

---

# 1. Core engineering principles

## 1.1 Manual before automation

Every important component must first be:

1. built manually;
2. validated manually;
3. documented;
4. deliberately broken;
5. troubleshot;
6. recovered;
7. only then automated.

Automation must encode understood behavior, not hide unknown behavior.

---

## 1.2 Evidence before status

A component is not considered DONE because configuration exists.

DONE requires evidence.

Examples:

- successful DNS query;
- successful domain join;
- `gpresult` showing expected policy;
- SMB access positive and negative tests;
- monitoring alert generated and resolved;
- successful backup;
- successful restore;
- successful rebuild from code;
- final baseline comparison.

---

## 1.3 Public / private separation

The public repository contains:

- documentation;
- reusable automation;
- synthetic addresses;
- example inventory;
- example configuration;
- tests;
- CI configuration;
- sanitized evidence.

The private environment store contains:

- real addressing;
- raw exports;
- credentials;
- secrets;
- real inventories;
- Terraform variables;
- protected state;
- backups;
- environment-specific mapping.

Raw operational data must never be copied into the public repository.

---

## 1.4 Existing HomeLab is a dependency, not project scope

The Enterprise Lab may use the existing HomeLab.

It must not silently redesign or clean unrelated legacy HomeLab state.

Existing technical debt is:

- documented;
- preserved;
- changed only if it directly blocks the Enterprise Lab and a separate change is approved.

---

## 1.5 Every destructive operation needs rollback

Before changes to:

- FortiGate;
- MikroTik;
- Proxmox networking;
- Active Directory;
- DNS;
- GPO;
- file permissions;
- automation state;

the project must define:

- expected result;
- validation;
- rollback path;
- recovery path;
- evidence.

---

# 2. High-level lifecycle

```text
BASELINE
   ↓
NETWORK FOUNDATION
   ↓
WINDOWS SERVER + AD
   ↓
WINDOWS CLIENTS + GPO
   ↓
RESILIENCE + FILE SERVICES
   ↓
LINUX INTEGRATION
   ↓
MONITORING
   ↓
PATCHING
   ↓
BACKUP / RESTORE
   ↓
FAILURE SCENARIOS
   ↓
AUTOMATION
   ↓
CI/CD
   ↓
AUTO-REMEDIATION
   ↓
DISASTER RECOVERY
   ↓
DESTROY
   ↓
REBUILD FROM CODE
   ↓
VERIFY
   ↓
FINAL DESTROY
   ↓
COMPARE WITH PRE-LAB BASELINE
```

---

# 3. Milestone model

The project uses milestones rather than a rigid calendar.

Suggested execution order:

| Milestone | Outcome |
| --- | --- |
| M0 | Baseline and live discovery |
| M1 | Enterprise network foundation |
| M2 | First Windows domain |
| M3 | Windows client and core GPO |
| M4 | Identity and policy depth |
| M5 | AD/DNS resilience |
| M6 | Enterprise file services |
| M7 | Linux integration |
| M8 | Monitoring and observability |
| M9 | Patch management |
| M10 | Backup and recovery |
| M11 | Controlled incident scenarios |
| M12 | PowerShell automation |
| M13 | Ansible configuration management |
| M14 | Terraform infrastructure provisioning |
| M15 | Network as Code |
| M16 | Secrets and environment management |
| M17 | CI |
| M18 | Private CD |
| M19 | Automated remediation |
| M20 | Disaster recovery automation |
| M21 | Full destroy/rebuild exercise |
| M22 | Final teardown and baseline comparison |
| M23 | Portfolio release |

---

# 4. M0 — PRE-LAB baseline and discovery

**Status:** COMPLETED enough to proceed.

## Objectives

Understand the real hosting environment before changing it.

## Tasks

- [x] Discover FortiGate interfaces, routes, DHCP, DNS, firewall policy and VIPs.
- [x] Discover Proxmox VLAN-aware bridge configuration.
- [x] Discover VM/LXC network dependencies.
- [x] Discover MikroTik bridge VLAN configuration.
- [x] Confirm physical trunk direction.
- [x] Capture private FortiGate baseline.
- [x] Capture MikroTik `.rsc` export.
- [x] Capture MikroTik binary backup.
- [x] Capture Proxmox baseline.
- [x] Record pre-existing technical debt.
- [x] Identify public/private documentation boundary.

## Findings

- Existing HomeLab routing remains on FortiGate.
- MikroTik is primarily L2 transport.
- Proxmox already supports tagged VM attachment.
- A pre-existing MikroTik management control-plane issue exists.
- That issue does not block Enterprise Lab data-plane operation.

## Exit gate

- [x] Baseline exists outside public Git.
- [x] Critical topology is understood.
- [x] Rollback source exists.
- [x] No unresolved discovery gap blocks first Enterprise Lab VLAN.

---

# 5. M1 — Enterprise network foundation

**Status:** COMPLETED.

## Objective

Create isolated server and client networks for the Enterprise Lab.

## Networks

Public documentation uses synthetic addressing.

- VLAN60 — `ENT-SERVERS`
- VLAN70 — `ENT-CLIENTS`

## Tasks

### FortiGate

- [x] Create server VLAN interface.
- [x] Create client VLAN interface.
- [x] Assign gateways.
- [x] Permit ICMP for validation.
- [x] Keep general policies absent until needed.

### MikroTik

- [x] Add VLAN60 tagged transport between FortiGate and Proxmox trunks.
- [x] Add VLAN70 tagged transport between FortiGate and Proxmox trunks.
- [x] Do not create unnecessary L3 interfaces.

### Proxmox

- [x] Confirm existing VLAN-aware bridge supports new VLAN IDs.
- [x] Avoid unnecessary host VLAN subinterfaces.

## Validation

- [x] Disposable VM tested in VLAN60.
- [x] VLAN60 gateway reachable.
- [x] Disposable VM moved to VLAN70.
- [x] VLAN70 gateway reachable.
- [x] 0% packet loss in both tests.

## Exit gate

- [x] End-to-end VLAN60 path proven.
- [x] End-to-end VLAN70 path proven.
- [x] HomeLab existing VLANs remain operational.

---

# 6. M2 — DC01 and first Active Directory forest

**Status:** IMPLEMENTED AND VALIDATED; CONTROLLED FAILURE DRILLS PENDING.

## Objective

Build the first Windows Server domain controller manually.

## Design decisions required

- [x] Select Windows Server version.
- [x] Select VM resources.
- [x] Accept AD DNS namespace.
- [x] Define static server addressing.
- [x] Define temporary outbound update policy.
- [x] Define DNS forwarding behavior.

## DC01 VM

Suggested starting point:

- 2–4 vCPU
- 4–8 GB RAM
- 60–80 GB disk
- VLAN60
- static IP
- no unnecessary extra NIC

## Tasks

- [x] Create DC01 VM manually.
- [x] Install Windows Server.
- [ ] Install updates.
- [x] Rename host to `DC01`.
- [x] Configure static IPv4.
- [x] Verify gateway reachability.
- [x] Verify time.
- [ ] Verify external name resolution before promotion.
- [x] Install AD DS role.
- [x] Install DNS role.
- [x] Create first forest.
- [x] Reboot.
- [x] Verify domain services.
- [x] Verify DNS zone.
- [x] Verify `_msdcs`.
- [x] Verify SRV records.
- [x] Run `dcdiag`.
- [x] Run basic DNS diagnostics.
- [ ] Document all manual actions.

## Validation

- [x] DC01 healthy.
- [x] AD DS services running.
- [x] DNS authoritative for AD namespace.
- [x] Required SRV records exist.
- [x] No critical `dcdiag` errors.
- [x] FortiGate gateway reachable.

## Failure scenarios

- [ ] Stop DNS service and observe domain impact.
- [ ] Configure wrong DNS temporarily and identify symptoms.
- [ ] Break time synchronization and observe Kerberos-related behavior.
- [ ] Restore correct configuration.

## Exit gate

DC01 must be stable before any domain client is joined.

---

# 7. M3 — WIN11-01 and first domain join

## Objective

Prove client-to-domain connectivity through the segmented network.

## Tasks

- [ ] Create Windows 11 VM.
- [ ] Attach to VLAN70.
- [ ] Configure client DNS to DC01 only.
- [ ] Verify DC01 DNS resolution.
- [ ] Verify AD SRV lookup.
- [ ] Join domain.
- [ ] Reboot.
- [ ] Sign in with domain account.
- [ ] Validate secure channel.
- [ ] Validate DNS registration.
- [ ] Validate authentication.

## Failure scenarios

### Wrong DNS

- [ ] Set client DNS to non-AD resolver.
- [ ] Attempt domain join.
- [ ] Record failure symptoms.
- [ ] Diagnose with `nslookup`.
- [ ] Restore DC DNS.
- [ ] Complete join.

### VLAN tag failure

- [ ] Intentionally use wrong VLAN tag.
- [ ] Observe connectivity failure.
- [ ] Diagnose from Proxmox and guest.
- [ ] Correct tag.

## Exit gate

- [ ] WIN11-01 joined.
- [ ] Domain authentication works.
- [ ] DNS behavior understood and documented.

---

# 8. M4 — OU, users, groups and Group Policy fundamentals

## Objective

Make GPO one of the central learning tracks.

## OU structure

Create a realistic but manageable hierarchy.

Example:

```text
Domain
├── Tier0
│   └── Domain Controllers
├── Servers
├── Workstations
│   ├── Standard
│   └── Test
├── Users
│   ├── IT
│   ├── Finance
│   └── Sales
└── Groups
```

Final structure must be justified in an ADR or design note.

## Identity tasks

- [ ] Create test users.
- [ ] Create role-based security groups.
- [ ] Practice AGDLP-style permission design.
- [ ] Practice group nesting.
- [ ] Practice delegated administration.
- [ ] Create test helpdesk delegation.
- [ ] Verify delegated user cannot perform privileged actions outside scope.

## GPO fundamentals

- [ ] Create workstation baseline GPO.
- [ ] Create user settings GPO.
- [ ] Link GPO to correct OU.
- [ ] Verify inheritance.
- [ ] Verify link order.
- [ ] Test Block Inheritance.
- [ ] Test Enforced.
- [ ] Test security filtering.
- [ ] Test loopback Merge.
- [ ] Test loopback Replace.
- [ ] Create GPP drive mapping.
- [ ] Test Item-Level Targeting.
- [ ] Configure selected Windows Firewall rules through GPO.
- [ ] Configure selected local security settings.
- [ ] Build ADMX Central Store.
- [ ] Review Microsoft security baseline concepts.
- [ ] Introduce Windows LAPS.

## Troubleshooting tools

- [ ] `gpupdate`
- [ ] `gpresult /r`
- [ ] `gpresult /h`
- [ ] RSoP
- [ ] Event Viewer → GroupPolicy/Operational
- [ ] PowerShell GPO cmdlets

## Failure scenarios

- [ ] GPO linked to wrong OU.
- [ ] GPO blocked by inheritance.
- [ ] Security filtering removes expected principal.
- [ ] Loopback misunderstanding.
- [ ] User policy placed where only computers exist.
- [ ] Conflicting GPO precedence.
- [ ] LAPS policy not applying.

## Exit gate

The operator must be able to answer:

- why a policy applied;
- why a policy did not apply;
- which policy won;
- how to prove it with tools.

---

# 9. M5 — DC02, replication and DNS resilience

## Objective

Move from a single-DC lab to a more realistic domain.

## Tasks

- [ ] Create DC02.
- [ ] Place in VLAN60.
- [ ] Configure static address.
- [ ] Join existing domain.
- [ ] Install AD DS.
- [ ] Promote as additional DC.
- [ ] Install DNS.
- [ ] Verify AD replication.
- [ ] Verify DNS replication.
- [ ] Verify SYSVOL replication.
- [ ] Verify both DCs advertise correctly.
- [ ] Configure clients to use DC01 and DC02 as DNS.
- [ ] Test authentication when DC01 is offline.
- [ ] Test DNS when DC01 is offline.

## Tools

- [ ] `repadmin /replsummary`
- [ ] `repadmin /showrepl`
- [ ] `dcdiag`
- [ ] DNS Manager
- [ ] Event Viewer
- [ ] PowerShell AD cmdlets

## Failure scenarios

- [ ] DC01 DNS stopped.
- [ ] DC01 powered off.
- [ ] Broken replication.
- [ ] Incorrect DNS client order.
- [ ] Time drift.
- [ ] SYSVOL/GPO replication issue.

## Important recovery rule

Do not treat VM snapshots as a universal DC recovery method.

AD recovery must be understood separately from ordinary VM rollback.

## Exit gate

- [ ] Domain functions with either DC unavailable.
- [ ] Replication health verified.
- [ ] Failure and recovery documented.

---

# 10. M6 — File Server, SMB and NTFS

## Objective

Build enterprise file access using groups and layered permissions.

## FILE01

- [ ] Create Windows Server VM.
- [ ] Join domain.
- [ ] Create data volume.
- [ ] Create SMB shares.

## Permission model

Practice:

- share permissions;
- NTFS permissions;
- inheritance;
- explicit deny;
- group-based access;
- least privilege;
- effective access.

Suggested model:

```text
Users → Global groups → Domain Local resource groups → ACL
```

## Tasks

- [ ] Create department shares.
- [ ] Create security groups.
- [ ] Assign NTFS ACLs.
- [ ] Configure share permissions.
- [ ] Map drives using GPP.
- [ ] Test access as multiple users.
- [ ] Verify effective permissions.
- [ ] Enable auditing for selected paths.

## Failure scenarios

- [ ] Share allows but NTFS denies.
- [ ] NTFS allows but share restricts.
- [ ] Nested group membership issue.
- [ ] Broken inheritance.
- [ ] Explicit deny overrides expected access.
- [ ] User receives wrong mapped drive.

## Exit gate

- [ ] Positive access tests pass.
- [ ] Negative access tests pass.
- [ ] Effective permission can be explained.

---

# 11. M7 — Linux domain integration

## Objective

Integrate Linux with Active Directory identity.

## Build

Create at least one Enterprise Linux VM.

## Tasks

- [ ] Configure DNS to AD DNS.
- [ ] Verify time synchronization.
- [ ] Install required Kerberos/SSSD/realmd packages.
- [ ] Discover domain.
- [ ] Join domain.
- [ ] Verify Kerberos ticket.
- [ ] Verify AD identity lookup.
- [ ] Authenticate with domain user.
- [ ] Restrict login by AD group.
- [ ] Configure sudo based on AD group where appropriate.
- [ ] Document SSSD configuration.
- [ ] Document key Kerberos configuration.

## Tools

- [ ] `realm`
- [ ] `kinit`
- [ ] `klist`
- [ ] `id`
- [ ] `getent`
- [ ] `sssctl`
- [ ] `journalctl`

## Failure scenarios

- [ ] Wrong DNS.
- [ ] Time drift.
- [ ] Broken Kerberos configuration.
- [ ] SSSD cache issue.
- [ ] Group authorization failure.

## Exit gate

Linux identity behavior must be diagnosable, not only functional.

---

# 12. M8 — Monitoring and observability

## Objective

Build monitoring gradually and finish with a complete alert lifecycle.

Do not start with a large stack.

Start with one complete path.

## Stage 1 — Core monitoring

Suggested first implementation:

- Zabbix Server
- agents
- SNMP where useful

Alternative technologies may later be compared.

## Initial monitored systems

- [ ] DC01
- [ ] DC02
- [ ] FILE01
- [ ] WIN11-01
- [ ] Linux server
- [ ] FortiGate
- [ ] Proxmox
- [ ] selected MikroTik metrics if appropriate

## Basic metrics

- CPU
- memory
- disk
- interface state
- service state
- availability
- latency
- packet loss

## Service-oriented checks

- [ ] DNS service state
- [ ] AD-related Windows services
- [ ] SMB availability
- [ ] Linux SSH
- [ ] selected HTTP endpoints
- [ ] Proxmox availability

## First complete monitoring lifecycle

```text
healthy
→ fault introduced
→ monitoring detects fault
→ PROBLEM
→ notification/event
→ troubleshooting
→ service restored
→ monitoring verifies recovery
→ RESOLVED
```

## Failure scenarios

- [ ] Stop DNS service.
- [ ] Stop monitored Linux service.
- [ ] Fill test filesystem.
- [ ] Disconnect test VM network.
- [ ] Trigger packet loss threshold.

## Later comparison

Only after the first monitoring system is understood:

- [ ] Prometheus
- [ ] exporters
- [ ] Grafana
- [ ] Alertmanager

The goal is to understand the architecture rather than install tools for their own sake.

## Exit gate

At least five incidents must generate reliable alerts and clear recovery evidence.

---

# 13. M9 — Patch and update management

## Objective

Understand the operational lifecycle of endpoints and servers.

## Windows clients

- [ ] Define update policy.
- [ ] Define reboot behavior.
- [ ] Test update installation.
- [ ] Test update history.
- [ ] Test failed update troubleshooting.
- [ ] Test rollback/uninstall where supported.

## Windows Server

- [ ] Define maintenance approach.
- [ ] Patch DCs one at a time.
- [ ] Verify AD/DNS health after patching.
- [ ] Patch file server.
- [ ] Verify SMB service.

## Linux

- [ ] Update package metadata.
- [ ] Apply updates.
- [ ] Identify services requiring restart.
- [ ] Reboot where needed.
- [ ] Verify post-update service state.

## Monitoring integration

- [ ] Detect systems requiring reboot.
- [ ] Detect failed services after update.
- [ ] Verify service recovery.

## Failure scenarios

- [ ] Update causes service stop.
- [ ] Client fails to complete update.
- [ ] Server reboot occurs in wrong order.
- [ ] DNS unavailable during patch window.

## Exit gate

Updates must be treated as controlled changes with pre-check and post-check.

---

# 14. M10 — Backup and restore

## Objective

Back up what matters and prove restoration.

A backup that has never been restored is not considered validated.

## Backup classes

### Configuration backups

- FortiGate
- MikroTik
- Proxmox relevant configuration
- automation configuration
- GPO backup

### VM backups

- Windows servers
- Linux servers
- selected test clients where useful

### Application/data backups

- file server data
- AD-specific recovery material where appropriate
- monitoring configuration
- automation repositories
- private environment data

## Tasks

- [ ] Define backup inventory.
- [ ] Define retention.
- [ ] Define storage location.
- [ ] Define encryption.
- [ ] Define integrity checking.
- [ ] Define restore test schedule.

## Recovery tests

### File-level restore

- [ ] Delete a test file.
- [ ] Restore it.
- [ ] Verify content.
- [ ] Verify ACL.

### Directory restore

- [ ] Delete a test directory.
- [ ] Restore it.
- [ ] Verify inheritance and permissions.

### VM restore

- [ ] Restore a non-DC VM.
- [ ] Validate networking.
- [ ] Validate service.

### GPO restore

- [ ] Back up GPO.
- [ ] Delete or modify test GPO.
- [ ] Restore.
- [ ] Verify application.

### Domain controller recovery study

- [ ] Document supported AD recovery approach.
- [ ] Understand system state concepts.
- [ ] Understand authoritative vs non-authoritative restore conceptually.
- [ ] Do not blindly revert DC snapshots as a generic recovery strategy.

## Exit gate

At least one successful restore must exist for every backup class used.

---

# 15. M11 — Incident and troubleshooting laboratory

## Objective

Turn the environment into a repeatable troubleshooting training platform.

Every scenario needs:

- incident description;
- symptoms;
- expected alerts;
- diagnosis path;
- commands/tools;
- root cause;
- remediation;
- verification;
- lesson learned.

## Network incidents

- [ ] Wrong VLAN tag.
- [ ] Missing MikroTik VLAN transport.
- [ ] Missing FortiGate interface.
- [ ] Missing firewall policy.
- [ ] Incorrect gateway.
- [ ] DNS blocked by policy.
- [ ] asymmetric/unexpected route experiment where safe.

## DNS incidents

- [ ] Client uses wrong DNS.
- [ ] DNS service stopped.
- [ ] Missing/incorrect forwarder.
- [ ] missing record.
- [ ] SRV lookup failure.

## AD incidents

- [ ] broken secure channel;
- [ ] replication failure;
- [ ] time/Kerberos issue;
- [ ] disabled account;
- [ ] incorrect group membership.

## GPO incidents

- [ ] wrong OU;
- [ ] inheritance problem;
- [ ] security filtering;
- [ ] WMI filter mistake;
- [ ] conflicting policies;
- [ ] loopback mistake.

## File service incidents

- [ ] share/NTFS conflict;
- [ ] deleted file;
- [ ] wrong group membership;
- [ ] broken inheritance.

## Linux incidents

- [ ] SSSD offline;
- [ ] Kerberos issue;
- [ ] wrong DNS;
- [ ] authorization group failure.

## Exit gate

The operator can troubleshoot from symptoms instead of following a memorized setup guide.

---

# 16. M12 — PowerShell automation

## Objective

Automate repetitive Windows administration after manual understanding exists.

## Areas

### Active Directory

- [ ] create OU structure;
- [ ] create users from data;
- [ ] create groups;
- [ ] assign memberships;
- [ ] export identity state;
- [ ] validate required objects.

### GPO

- [ ] inventory GPOs;
- [ ] back up GPOs;
- [ ] restore selected GPOs;
- [ ] produce HTML/XML reports;
- [ ] validate expected links.

### Windows Server

- [ ] configure selected roles/features;
- [ ] collect health information;
- [ ] validate services;
- [ ] collect event logs.

### Clients

- [ ] collect `gpresult`;
- [ ] collect update state;
- [ ] validate domain membership.

## Engineering requirements

Every script must support where appropriate:

- meaningful exit code;
- logging;
- idempotent behavior where possible;
- validation;
- dry-run / `-WhatIf` where feasible;
- clear input/output.

## Exit gate

No script is accepted only because it ran once.

---

# 17. M13 — Ansible configuration management

## Objective

Automate Linux and selected Windows configuration.

## Repository structure

Develop:

- inventory examples;
- roles;
- group vars;
- host vars templates;
- tests;
- documented secrets boundary.

## Linux roles

Possible roles:

- common
- time
- dns_client
- monitoring_agent
- ad_join
- ssh
- packages

## Windows

Where appropriate:

- WinRM setup
- selected Windows configuration
- monitoring agent
- feature configuration
- file server configuration components

## Requirements

- [ ] idempotence checked;
- [ ] secrets not committed;
- [ ] second run produces no unexpected change;
- [ ] failed host does not corrupt others;
- [ ] validation tasks exist.

## Exit gate

Manual Linux configuration can be reproduced from automation.

---

# 18. M14 — Terraform / Proxmox

## Objective

Provision the VM layer reproducibly.

## Scope

Terraform owns infrastructure objects such as:

- VM existence;
- VM resources;
- disk;
- NIC;
- VLAN tag;
- cloud-init where appropriate;
- selected Proxmox pool/project organization.

Terraform does not own application configuration that belongs to Ansible or PowerShell.

## Tasks

- [ ] define provider;
- [ ] define example variables;
- [ ] define VM modules;
- [ ] define DC VM resources;
- [ ] define client VM resources;
- [ ] define Linux VM resources;
- [ ] define monitoring VM resources;
- [ ] define outputs;
- [ ] define state boundary;
- [ ] protect real state privately.

## Workflow

```text
terraform fmt
→ validate
→ plan
→ review
→ apply
→ post-deploy tests
```

## Drift

- [ ] create controlled manual drift;
- [ ] detect it;
- [ ] decide whether Terraform should correct or preserve;
- [ ] document ownership.

## Exit gate

A clean VM set can be recreated without manual Proxmox GUI work.

---

# 19. M15 — Network as Code

## Objective

Represent ENT-LAB network changes as reusable code.

Do not automate unrelated legacy HomeLab configuration.

## Common desired data model

Example concepts:

```yaml
networks:
  ent_servers:
    vlan_id: 60
    subnet: <private>
    gateway: <private>
  ent_clients:
    vlan_id: 70
    subnet: <private>
    gateway: <private>
```

The public repository contains example values only.

## FortiGate automation

Candidate technologies:

- Terraform FortiOS provider
- Ansible Fortinet collection

Automate only objects clearly owned by ENT-LAB:

- VLAN interfaces
- address objects
- DHCP scopes if used
- policies
- selected service objects

## MikroTik automation

Candidate technologies:

- RouterOS API
- REST/API automation
- Ansible RouterOS collection
- reusable RouterOS scripts where appropriate

Automate:

- ENT-LAB VLAN bridge entries
- comments/labels
- validation

## Safety requirements

Before network apply:

- backup configuration;
- verify management path;
- produce change plan;
- define rollback;
- avoid touching unrelated objects.

After apply:

- gateway validation;
- VLAN path test;
- management reachability test;
- configuration diff.

## Exit gate

ENT-LAB network foundation can be removed and recreated without manual CLI configuration.

---

# 20. M16 — Secrets, state and private environment data

## Objective

Separate reusable code from real environment values.

## Public repository

Contains:

- example variables;
- example inventories;
- templates;
- documentation.

## Private environment repository/store

Contains:

- real IPs;
- inventories;
- Terraform variables;
- state backend configuration;
- encrypted secrets;
- deployment metadata.

## Secrets technology

Evaluate:

- SOPS + age
- Vault
- another justified secret store

## Rules

- no plaintext credentials in Git;
- no private keys in public repo;
- no unencrypted Terraform state with secrets in public locations;
- rotation procedure documented;
- access minimized.

## Exit gate

A new operator can understand where configuration ends and secret/environment data begins.

---

# 21. M17 — Continuous Integration

## Objective

Validate changes before deployment.

Public-safe CI may run immediately without infrastructure credentials.

## Public CI checks

- [ ] Markdown lint
- [ ] YAML lint
- [ ] JSON validation
- [ ] link checking
- [ ] secret scanning
- [ ] Terraform format
- [ ] Terraform validate with safe/example values
- [ ] Ansible syntax check
- [ ] PowerShell lint / PSScriptAnalyzer
- [ ] custom schema checks
- [ ] documentation consistency checks

## Security boundary

Public PR code must not automatically run on a self-hosted runner holding infrastructure secrets.

## Exit gate

Bad formatting, obvious secrets and invalid automation are rejected before merge.

---

# 22. M18 — Private Continuous Deployment

## Objective

Deploy approved infrastructure changes through controlled automation.

## Architecture

```text
Public reusable code
       ↓
review / merge
       ↓
private environment source
       ↓
private/self-hosted runner
       ↓
plan
       ↓
approval
       ↓
deployment
       ↓
post-deploy verification
```

## Requirements

- least-privilege credentials;
- environment isolation;
- explicit approval for destructive changes;
- logs;
- artifact retention;
- rollback procedure;
- no untrusted public PR execution with privileged credentials.

## Deployment stages

1. static validation;
2. plan;
3. human approval;
4. apply;
5. health checks;
6. failure handling;
7. evidence.

## Exit gate

A normal approved change can move from Git to infrastructure without manual configuration.

---

# 23. M19 — Automated incident detection and remediation

## Objective

Move from monitoring to controlled self-healing.

Automation must be risk-tiered.

Blind self-healing is not permitted for all systems.

---

## 23.1 Incident lifecycle

```text
DETECT
  ↓
CLASSIFY
  ↓
COLLECT EVIDENCE
  ↓
DECIDE REMEDIATION CLASS
  ↓
AUTO-REMEDIATE OR REQUEST APPROVAL
  ↓
VERIFY
  ↓
ESCALATE IF FAILED
  ↓
DOCUMENT
```

---

## 23.2 Recovery classes

### Class A — Safe automatic remediation

Can run automatically when narrowly scoped and verified.

Examples:

- restart a known stateless service;
- restart monitoring agent;
- restart a failed test application;
- renew a safe agent connection;
- restart a disposable lab VM;
- rerun a failed idempotent configuration role;
- reapply known-good configuration to a stateless Linux service.

Requirements:

- bounded retries;
- before/after evidence;
- health check;
- timeout;
- escalation after failure.

---

### Class B — Automatic remediation with rollback

Automation may apply a known change and automatically revert if health checks fail.

Examples:

- application configuration deployment;
- selected Linux configuration;
- selected Windows configuration;
- selected ENT-LAB firewall object changes;
- selected VLAN changes after management path safety is proven.

Pattern:

```text
capture state
→ apply
→ health check
→ success: commit result
→ failure: rollback
→ verify rollback
→ alert
```

---

### Class C — Approval-gated recovery

Detection and diagnosis may be automated.

Recovery requires approval.

Examples:

- Active Directory restore;
- domain controller rebuild;
- DNS authority changes;
- file data restore;
- firewall policy rollback affecting broad connectivity;
- MikroTik bridge changes that could remove management;
- credential rotation;
- destructive Terraform operations.

Automation prepares:

- incident summary;
- affected systems;
- proposed action;
- expected impact;
- rollback;
- commands/plan.

Human approves execution.

---

### Class D — Manual recovery only

Used where automation risk is higher than expected recovery benefit.

Initially includes:

- unknown AD database corruption;
- ambiguous security incident;
- suspected credential compromise;
- unexplained data loss;
- multi-system failure with uncertain dependency state.

These may later move to Class C after learning.

---

# 24. Automated recovery use cases

## 24.1 Windows service failure

Example:

DNS-related test service or selected non-critical Windows service fails.

Flow:

1. Monitoring detects stopped service.
2. Automation collects service status and recent events.
3. If incident matches approved Class A rule:
   - start service;
   - wait;
   - verify service;
   - run functional test.
4. If healthy:
   - close incident as auto-remediated.
5. If unhealthy:
   - escalate.

For core AD/DNS services, automatic restart may be allowed only after explicit testing and guardrails.

---

## 24.2 Linux service failure

Flow:

```text
alert
→ collect journal
→ restart service
→ verify socket/HTTP check
→ success or escalate
```

Bounded retries only.

---

## 24.3 Disposable VM failure

If VM is expected to be stateless or reproducible:

```text
detect unavailable VM
→ confirm host healthy
→ attempt VM restart
→ health check
→ if failed:
      recreate from Terraform
      configure with Ansible
      verify
```

---

## 24.4 Configuration drift

Flow:

```text
scheduled drift detection
→ identify changed owned object
→ compare desired state
→ classify
→ auto-correct safe drift
   OR
→ create approval request for risky drift
```

Never correct objects outside automation ownership.

---

## 24.5 Failed deployment

Deployment pipeline:

```text
pre-check
→ deployment
→ post-check
```

If post-check fails:

```text
stop further stages
→ automatic rollback where supported
→ verify previous state
→ create incident record
```

---

## 24.6 Network incident

Examples:

- VLAN entry missing;
- owned firewall rule missing;
- wrong lab gateway object.

Initial behavior:

- detect;
- collect config;
- produce diff;
- prepare remediation.

Only after repeated safe testing may selected lab-only objects receive automatic correction.

Management-path-affecting changes remain approval-gated.

---

# 25. M20 — Disaster recovery automation

## Objective

Recover the environment when individual services or the entire lab is lost.

The emphasis is reproducibility, not permanent HA.

---

## 25.1 Recovery hierarchy

### Level 1 — Service recovery

Restart/reconfigure service.

### Level 2 — VM recovery

Restart or restore/recreate VM.

### Level 3 — Server rebuild

Recreate VM from Terraform and configuration from Ansible/PowerShell.

### Level 4 — Application/data restore

Restore required data/configuration from validated backup.

### Level 5 — Complete Enterprise Lab rebuild

Recreate the lab from:

- private environment data;
- Terraform;
- Ansible;
- PowerShell;
- network automation;
- protected backups where required.

---

## 25.2 Full recovery controller concept

Target orchestration:

```text
Incident / operator trigger
        ↓
Recovery orchestrator
        ↓
Read environment desired state
        ↓
Validate HomeLab prerequisites
        ↓
Ensure VLAN/network objects
        ↓
Ensure Proxmox VMs
        ↓
Apply server configuration
        ↓
Restore required data
        ↓
Run AD/DNS validation
        ↓
Run client validation
        ↓
Run monitoring validation
        ↓
Generate recovery report
```

---

## 25.3 Recovery dependency order

Recommended order:

1. HomeLab base connectivity
2. FortiGate ENT-LAB networking
3. MikroTik ENT-LAB VLAN transport
4. Proxmox VM layer
5. DC01 / DNS
6. DC02
7. identity validation
8. file services
9. Linux identity integration
10. monitoring
11. clients
12. non-critical applications

Dependencies must be encoded and documented.

---

## 25.4 Automated validation after recovery

Recovery is not complete until tests pass.

Tests include:

### Network

- VLAN60 gateway reachable
- VLAN70 gateway reachable
- required inter-VLAN paths pass
- denied paths remain denied

### AD

- DC services healthy
- replication healthy
- domain lookup works
- authentication works

### DNS

- AD zone resolves
- SRV records resolve
- external forwarding works

### GPO

- test client gets expected policies
- `gpresult` includes baseline policy

### File services

- share reachable
- allowed user succeeds
- denied user fails

### Linux

- domain user lookup works
- allowed login works
- denied login fails

### Monitoring

- hosts return to healthy
- no unresolved recovery-created alert remains

---

# 26. Recovery targets

These are training targets, not production SLAs.

They should be measured and revised.

Example initial targets:

| Recovery scope | Initial training target |
| --- | --- |
| Restart stateless service | < 5 min |
| Restore/recreate simple Linux VM | < 15 min |
| Recreate Windows member server | < 30 min |
| Restore selected file/data set | < 30 min |
| Restore domain service after single-server failure | < 30 min |
| Rebuild complete Enterprise Lab | < 120 min |

Record actual observed recovery time.

Do not falsify targets to make the project look better.

---

# 27. M21 — Full destroy and rebuild exercise

## Objective

Prove that documentation and automation can recreate the environment.

## Pre-destroy gate

- [ ] backups validated;
- [ ] environment state stored;
- [ ] required secrets accessible;
- [ ] code merged;
- [ ] CI green;
- [ ] recovery runbook reviewed;
- [ ] underlying HomeLab baseline protected.

## Destroy

Remove only ENT-LAB-owned objects.

Examples:

- Enterprise VMs
- Enterprise VLAN interfaces
- Enterprise policies
- Enterprise MikroTik VLAN entries
- Enterprise DHCP objects
- Enterprise automation state as planned

Do not remove unrelated HomeLab objects.

## Rebuild

Recreate:

1. network;
2. VM infrastructure;
3. AD/DNS;
4. Windows members;
5. file services;
6. Linux;
7. monitoring;
8. policies;
9. clients.

## Validation

Run automated test suite.

## Exit gate

The rebuilt environment must satisfy the same acceptance tests as the manually built environment.

---

# 28. M22 — Final teardown

## Objective

Remove Enterprise Lab cleanly when learning goals are complete.

## Tasks

- [ ] export final sanitized evidence;
- [ ] preserve code;
- [ ] preserve runbooks;
- [ ] preserve incident scenarios;
- [ ] preserve recovery metrics;
- [ ] preserve architecture diagrams;
- [ ] destroy ENT-LAB resources;
- [ ] remove ENT-LAB firewall/network objects;
- [ ] remove ENT-LAB VLAN transport;
- [ ] verify existing HomeLab services;
- [ ] capture post-lab configuration state;
- [ ] compare against PRE-LAB baseline.

## Final baseline diff

Classify differences:

- expected HomeLab hygiene change;
- intentional permanent improvement;
- accidental drift;
- unresolved difference.

Every unexpected difference must be explained or reverted.

## Exit gate

The Enterprise Lab no longer depends on running infrastructure.

Documentation and code remain.

---

# 29. M23 — Portfolio release

## Objective

Turn the engineering work into a clear public proof of skill.

## README should show

- project purpose;
- architecture diagram;
- technology stack;
- lifecycle;
- what was manually built;
- what was broken and recovered;
- what was automated;
- recovery architecture;
- final reproducibility result.

## Evidence

Include sanitized examples of:

- network test results;
- AD health checks;
- GPO reports;
- monitoring alerts;
- backup restore evidence;
- Terraform plan;
- Ansible idempotence;
- CI validation;
- recovery execution;
- full rebuild timing.

## Interview narrative

The project should support a concise explanation:

> I built an isolated enterprise environment manually on my HomeLab, including VLAN segmentation, Active Directory, DNS, Group Policy, Windows and Linux integration, file services, monitoring, patching and backup. I deliberately introduced failures, documented troubleshooting, then automated provisioning and configuration with Terraform, Ansible and PowerShell. Finally I implemented monitored recovery workflows, destroyed the environment, rebuilt it from code, validated it automatically and removed it without leaving unexpected HomeLab drift.

---

# 30. Cross-project Definition of Done

The project is complete only when all of the following are true.

## Infrastructure

- [ ] enterprise network reproducible;
- [ ] AD domain reproducible;
- [ ] Windows clients reproducible;
- [ ] Linux integration reproducible;
- [ ] file services reproducible;
- [ ] monitoring reproducible.

## Operations

- [ ] updates tested;
- [ ] backups tested;
- [ ] restores tested;
- [ ] incidents documented;
- [ ] failure scenarios repeated successfully.

## Automation

- [ ] PowerShell used for appropriate Windows automation;
- [ ] Ansible used for configuration management;
- [ ] Terraform used for infrastructure provisioning;
- [ ] network automation implemented for owned lab objects;
- [ ] secrets separated;
- [ ] CI validates code;
- [ ] private CD performs controlled deployment.

## Recovery

- [ ] selected Class A incidents auto-remediate;
- [ ] failed remediation escalates safely;
- [ ] risky recovery is approval-gated;
- [ ] VM/server rebuild works;
- [ ] full lab rebuild works;
- [ ] automated verification runs after recovery.

## Reproducibility

- [ ] environment destroyed;
- [ ] environment rebuilt;
- [ ] acceptance tests pass after rebuild;
- [ ] final environment destroyed again.

## HomeLab integrity

- [ ] post-lab state compared with PRE-LAB baseline;
- [ ] unexpected drift resolved.

## Portfolio

- [ ] public repository contains no environment secrets;
- [ ] documentation reflects implemented reality;
- [ ] evidence exists;
- [ ] project can be explained without relying on ChatGPT conversation history.

---

# 31. Automation ownership model

Avoid multiple tools fighting over the same object.

Suggested ownership:

| Object | Primary owner |
| --- | --- |
| Proxmox VM lifecycle | Terraform |
| VM CPU/RAM/disk/NIC/VLAN | Terraform |
| Linux OS configuration | Ansible |
| Linux domain join | Ansible |
| Windows AD objects | PowerShell |
| GPO lifecycle/reporting | PowerShell |
| Windows configuration | PowerShell / Ansible where justified |
| FortiGate ENT-LAB objects | Terraform or Ansible — choose one primary owner |
| MikroTik ENT-LAB VLAN objects | Ansible/API/script — choose one primary owner |
| Monitoring configuration | configuration management / monitoring API |
| Secrets | dedicated secret-management layer |
| Deployment orchestration | CI/CD |
| Recovery orchestration | runbook automation / private pipeline |

Ownership decisions must be recorded before automation implementation.

---

# 32. Incident record template

Each controlled or real lab incident should record:

```markdown
# Incident: <name>

Date:
Severity:
Detection:
Affected service:

## Symptoms

## Monitoring evidence

## Initial hypothesis

## Diagnostics performed

## Root cause

## Remediation

## Verification

## Recovery time

## Was remediation automatic?
- yes / no / approval-gated

## Automation opportunity

## Prevention / guardrail

## Lessons learned
```

---

# 33. Change gate template

Before meaningful infrastructure change:

```markdown
## Change

### Goal

### Scope

### Dependencies

### Risk

### Backup / baseline available
- [ ] yes

### Rollback

### Validation

### Negative tests

### Approval required
- [ ] no
- [ ] self-review
- [ ] explicit manual approval

### Result

### Evidence
```

---

# 34. Session workflow

At the beginning of a work session:

1. read `STATUS.md`;
2. read the current phase in this MASTER PLAN;
3. execute only the next approved slice;
4. avoid unrelated cleanup.

At the end:

1. update `STATUS.md`;
2. update relevant architecture/runbook documents;
3. save private operational evidence;
4. update checkboxes in this MASTER PLAN only when evidence exists;
5. commit public-safe changes;
6. set exactly one clear NEXT ACTION.

---

# 35. Current NEXT ACTION

The network foundation and the manual DC01 build are validated.

Proceed to:

## `M2 controlled failure drills`

Immediate sequence:

1. stop the DC DNS service and collect symptoms;
2. restore DNS and verify recovery;
3. introduce an incorrect DNS configuration and diagnose it;
4. restore the known-good DNS configuration;
5. deliberately disturb time synchronization and collect diagnostic evidence;
6. restore the approved PDC time configuration;
7. rerun DC, DNS, SYSVOL, Netlogon and time-health validation;
8. update public-safe documentation and close M2.

After the M2 recovery drills pass, proceed to `M3 — WIN11-01 and first domain join`.

Do not begin Terraform/Ansible automation of DC01 until the manual build,
failure behavior and recovery procedure are understood.
