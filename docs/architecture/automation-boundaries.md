# Future automation boundaries

**PROPOSED TARGET STATE — ownership and tooling choices remain OPEN DECISION.** No executable automation is included.

## Intended responsibility split

| Area | Candidate owner | Review needed |
| --- | --- | --- |
| Proxmox guest lifecycle | `terraform/proxmox/` | Provider/version, supported resources, import/adoption, drift/state and template ownership |
| FortiGate declarative objects | `terraform/fortigate/` | Provider coverage, existing-object adoption, policy order, NAT/VIP behavior and recovery risk |
| FortiGate bootstrap/recovery intent | `network/fortigate/` | Initial management access, firmware, handoff to steady-state owner, protected export handling |
| MikroTik configuration | `network/mikrotik/` | RouterOS version, API/tooling choice, bridge/VLAN adoption and safe restore method |
| OS/service orchestration | `ansible/roles/` | Linux roles, Windows remoting/bootstrap transport, dependencies and idempotence |
| Windows-specific tasks/checks | `powershell/` | Division with Ansible, invocation contract, permissions, output redaction and rollback |
| Verification | `tests/`, later Zabbix | Read-only versus disruptive tests, test data and protected evidence |

A directory reservation does not select a provider. A single object must not be managed concurrently by Terraform, vendor scripts and Ansible. Review bootstrap-to-steady-state handoffs and import/adoption before enabling reconciliation. API credentials, state backend and real inventory stay external.

## Required delivery sequence

1. Git holds reusable code and synthetic examples.
2. CI validates trusted formats and design contracts; public/untrusted jobs have no infrastructure access.
3. A trusted planning stage uses protected environment inputs; real plans/state remain private.
4. Approval identifies the exact revision and plan. Changed inputs or code require replanning and renewed approval.
5. Private/self-hosted execution applies the approved network/Proxmox change and then Windows/Linux configuration in the reviewed dependency order.
6. Verification and monitoring establish expected behavior and trigger a reviewed response if acceptance fails.

Runner placement, connectivity, identities, isolation, concurrency/locking, state backend, secret store, artifact retention, provider pins, drift policy and approval mechanism remain unresolved. No public PR job may execute on a privileged self-hosted runner.

## Replacement-hardware recovery contract

Future recovery documentation must cover: compatible same-model hardware/firmware; licenses or entitlements where required; console/local access; minimal bootstrap networking; protected identity/interface mappings; obtaining secrets and backups without depending on the failed device; ordered configuration restoration; management lockout prevention; validation of trunks, DHCP, routing, firewall policy and NAT/VIPs; and return to a single steady-state owner.

Protected vendor backups may support emergency recovery but do not replace the reusable-code requirement. Exact serials, MACs, credentials and environment-specific mappings stay external. Recovery objectives and the split between unavoidable bootstrap steps and automated restoration must be accepted before claiming reproducibility.
