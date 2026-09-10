# Mixed Enterprise Infrastructure Lab

An enterprise-style Infrastructure/Network-as-Code learning and portfolio project on Proxmox, integrating network engineering, Windows identity, Linux services, observability, and reproducible operations.

**Status: architecture and documentation only. Target Architecture review is the next milestone. Nothing in this repository deploys or configures infrastructure.**

## Engineering approach

Manual configuration → documentation → troubleshooting/failure scenarios → automation → reproducibility.

Each future component must show how it works, how it fails, how it is diagnosed, and how it can be rebuilt. A directory name is a responsibility reservation, not evidence of an implemented capability.

## Start here

1. [PROJECT.md](PROJECT.md): project status, phases, decisions, and definition of done.
2. [Current state](docs/architecture/current-state.md): sanitized confirmed baseline.
3. [Target state](docs/architecture/target-state.md): proposals and unresolved design choices.
4. [Target Architecture review](docs/architecture/target-review.md): milestone acceptance checklist.
5. [Repository map](docs/repository-map.md): responsibility of every file and directory.
6. [Backlog](docs/project/backlog.md): review work and dependencies.
7. [Security rules](SECURITY.md): publication and environment-data boundary.

## Status vocabulary

| Label | Meaning |
| --- | --- |
| CONFIRMED CURRENT STATE | Explicitly supplied architecture facts; not independently audited by this repository. |
| PROPOSED TARGET STATE | A candidate future design; not deployed and not approved. |
| OPEN DECISION | A choice or missing fact that needs evidence and review. |
| ACCEPTED CONSTRAINT | A project rule explicitly required by the architecture checkpoint. |

Example addresses and names are synthetic documentation values even in confirmed-state documents. They never assert a real addressing plan. All example networks use `10.250.x.0/24`; `corp.example.test` is an example namespace only. No public-to-private address mapping is included.

## Eventual scope

| Area | Intended coverage |
| --- | --- |
| Virtualization | Proxmox VE, templates, tagged VM networking, lifecycle and recovery |
| Network | FortiGate, MikroTik RouterOS, VLANs, routing, firewall policy, DHCP, NAT/VIPs |
| Windows | Windows Server, Active Directory, AD-integrated DNS, Group Policy, Windows 11, File Server, SMB/NTFS permissions, PowerShell |
| Linux | Linux servers, BIND9, Unbound, Kerberos/SSSD, Ansible |
| Operations | Zabbix, troubleshooting scenarios, runbooks, verification and restore exercises |
| Delivery | Git, Terraform, Ansible, CI validation, reviewed plans, approval, private/self-hosted execution |

These are project objectives, not completed deployments. Product/provider versions, licensing, capacity, and support requirements must be evaluated during design; no compatibility claim is made here.

## Repository layout

| Path | Responsibility |
| --- | --- |
| `docs/` | Architecture, network intent, ADRs, backlog, operational and failure-scenario templates |
| `data/example/` | Synthetic design examples; never a live inventory |
| `terraform/` | Reserved declarative infrastructure ownership, pending provider/resource review |
| `ansible/` | Reserved configuration orchestration and reusable roles |
| `powershell/` | Reserved Windows configuration and verification tooling |
| `network/` | Vendor-specific intent, bootstrap and restore design |
| `tests/` | Future acceptance/verification coverage and test responsibilities |
| `.github/` | Review templates and future CI responsibility; no executable workflow |

## Working with this skeleton

The source archive intentionally excludes Git metadata. After extraction, initialize a local repository with `git init -b main` if needed. Review files and configure the intended public commit identity locally before making a commit. No remote, hosting platform settings, branch protections, or publishing action are created by this deliverable.

Use [CONTRIBUTING.md](CONTRIBUTING.md) for changes. Review the complete staged diff and metadata against [SECURITY.md](SECURITY.md) before committing. `.gitignore` reduces accidental inclusion; it does not enforce secrecy or remove tracked data.

**Next milestone:** complete and review the Target Architecture, including segmentation, DNS coexistence, administrative access, migration/rollback, and automation ownership. DC01, VLAN60, and automation implementation remain outside the current phase.
