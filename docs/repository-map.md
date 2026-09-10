# Repository responsibility map

Documents are authoritative by topic: PROJECT for phase/gates; backlog for work status; current-state for confirmed facts; target-state and network plans for candidate design; ADRs for accepted/rejected rationale; SECURITY for the publication boundary. Update related documents together when a decision changes.

Every directory contains a README or a substantive child document so Git preserves the intended structure. No `.gitkeep` is needed. Implementation paths contain documentation only.

| File | Responsibility |
| --- | --- |
| `README.md` | Portfolio entry point, scope, status, navigation and archive/Git initialization guidance |
| `PROJECT.md` | Goal, principles, phases, discovery, current gate, next actions, decisions, debt, security and definitions of done |
| `CHANGELOG.md` | Versioned record of repository/design changes and honest deployment status |
| `.gitignore` | Defense-in-depth exclusions for private inputs, state/plans, exports and local outputs |
| `SECURITY.md` | Allowed/prohibited content, private boundary, publication and future CI trust rules |
| `CONTRIBUTING.md` | Proposed branch/review process, evidence standard and architecture-only contribution scope |
| `docs/repository-map.md` | File/directory responsibility index and authoritative-document boundaries |
| `docs/architecture/current-state.md` | Sanitized current-state checkpoint from supplied context plus live discovery, preserved legacy state and remaining gaps |
| `docs/architecture/target-state.md` | Unapproved segmentation/service design, accepted constraints and unresolved requirements |
| `docs/architecture/target-review.md` | Blocking M1 acceptance checklist and unfilled review record |
| `docs/architecture/automation-boundaries.md` | Future ownership, CI trust, bootstrap handoff and replacement recovery requirements |
| `docs/network/vlan-ip-plan.md` | Confirmed VLAN roles, synthetic addressing, candidate VLANs and allocation decisions |
| `docs/network/firewall-matrix.md` | Proposed flow intent, isolation requirements and rule review/verification criteria |
| `docs/network/dns-flow.md` | Confirmed DNS facts, discovery questions, coexistence choices and rollback requirements |
| `docs/adr/README.md` | ADR lifecycle and decision index |
| `docs/adr/template.md` | Reusable decision record format |
| `docs/adr/0001-public-private-boundary.md` | Accepted explicit public/private-data constraint |
| `docs/adr/0002-architecture-before-implementation.md` | Accepted explicit review-before-implementation constraint |
| `docs/adr/0003-dns-coexistence-proposed.md` | Unresolved DNS coexistence decision and required evidence |
| `docs/project/backlog.md` | Work IDs, dependencies, responsible roles and acceptance criteria |
| `docs/operations/README.md` | Operational runbook/evidence ownership |
| `docs/operations/runbook-template.md` | Future manual procedure, verification, abort/rollback and execution record |
| `docs/troubleshooting/README.md` | Failure-scenario scope and planned learning coverage |
| `docs/troubleshooting/scenario-template.md` | Reusable hypothesis/diagnosis/recovery evidence record |
| `data/README.md` | External environment-data boundary |
| `data/example/README.md` | Synthetic example semantics and non-deployable status |
| `data/example/architecture.example.json` | Machine-readable documentation example with explicit proposal/fact/address labels |
| `terraform/README.md` | Overall future IaC responsibility and unresolved provider/state decisions |
| `terraform/proxmox/README.md` | Reserved Proxmox lifecycle ownership and legacy protection requirements |
| `terraform/fortigate/README.md` | Reserved FortiGate declarative scope and bootstrap/steady-state ownership boundary |
| `ansible/README.md` | Future orchestration boundary and private inventory requirements |
| `ansible/roles/README.md` | Future role contract, candidate service areas and idempotence/verification expectations |
| `powershell/README.md` | Future Windows tooling scope and Ansible ownership contract |
| `network/README.md` | Shared vendor engineering, bootstrap and recovery responsibilities |
| `network/fortigate/README.md` | FortiGate manual/bootstrap/restore design boundary |
| `network/mikrotik/README.md` | RouterOS configuration/bootstrap/restore boundary and tooling decision |
| `tests/README.md` | Planned validation, functional/isolation checks and recovery evidence; no tests implemented |
| `.github/workflows/README.md` | Future CI stages and trust boundaries; no executable workflow |
| `.github/pull_request_template.md` | Architecture change and privacy review checklist |
| `.github/ISSUE_TEMPLATE/architecture-review.md` | Sanitized design-review issue format |

## Directory ownership

| Directory | Responsibility |
| --- | --- |
| Repository root | Project entry, governance and public safety rules |
| `docs/` | Human-readable design and operations evidence |
| `docs/architecture/` | Baseline, target, review gates and cross-tool boundaries |
| `docs/network/` | Vendor-independent VLAN/address/flow/DNS intent |
| `docs/adr/` | Decision history and rationale |
| `docs/project/` | Work planning and dependencies |
| `docs/operations/` | Manual build, maintenance and recovery procedures |
| `docs/troubleshooting/` | Controlled failure learning and sanitized causal evidence |
| `data/`, `data/example/` | Documentation-only synthetic inputs; no actual environment inventory |
| `terraform/`, `terraform/proxmox/`, `terraform/fortigate/` | Reserved infrastructure lifecycle ownership after review |
| `ansible/`, `ansible/roles/` | Reserved reusable OS/service orchestration |
| `powershell/` | Reserved Windows-specific administration and verification |
| `network/`, `network/fortigate/`, `network/mikrotik/` | Vendor bootstrap, manual knowledge and recovery design |
| `tests/` | Future verification contracts and evidence expectations |
| `.github/`, `.github/ISSUE_TEMPLATE/` | Review collaboration templates; no host configuration implied |
| `.github/workflows/` | Future workflow responsibility, currently inactive |

License selection is an OPEN DECISION; no license or copyright-owner identity has been invented. Remote publication, Git author identity and hosting protections are deliberately left for the publication work item.
