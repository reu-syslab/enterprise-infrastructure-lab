# M1 — Target Architecture review

Status: **OPEN DECISION — review not completed**. Responsible role: project owner, with architecture/security reviewer responsibilities; individuals not yet assigned.

An accepted review must reference the exact document revision, ADRs and sanitized evidence. Do not mark a check complete because a placeholder exists.

## Blocking checklist

- [ ] Baseline gaps that affect pilot safety are resolved: DNS flows, management access, relevant trunk/bridge dependencies and capacity.
- [ ] Enterprise segment purposes, VLAN IDs and address allocation method are accepted; Monitoring/Test colocation is explicitly decided.
- [ ] New routing/DHCP ownership, trunk/tag behavior, management reachability and recovery path are reviewed.
- [ ] AD namespace/topology, DC placement, time hierarchy and DNS authority/forwarding are accepted.
- [ ] Existing BIND9/Unbound consumers, loop avoidance, resolver access controls, DHCP DNS distribution, cutover and rollback are defined.
- [ ] Firewall matrix includes direction, purpose, exact required protocols, policy/NAT implications, positive and negative tests; no unintended AD WAN exposure.
- [ ] Pilot Windows client and GPO boundaries are defined; primary MGMT workstation excluded from first-client role.
- [ ] File Server SMB/NTFS permission model, Linux Kerberos/SSSD access and recovery, Zabbix scope and placement are decided to the level needed for the pilot.
- [ ] Versions, licensing, capacity, storage and backup assumptions are documented and validated for proposed use.
- [ ] Migration order, maintenance/recovery access, abort conditions and rollback success criteria are reviewed.
- [ ] Future object ownership, Terraform state protection, secret delivery, CI trust boundary and plan approval binding are documented; implementation deferred.
- [ ] Network replacement bootstrap/restore prerequisites and intended recovery objectives are documented.
- [ ] All blocking ADRs are accepted; remaining deferrals identify rationale, responsible role and later gate.
- [ ] Public documentation and evidence meet SECURITY.md; real mapping remains external.

## Review record (unfilled)

| Field | Value |
| --- | --- |
| Reviewed source revision | Pending |
| Architecture reviewer role | Pending assignment |
| Security reviewer role | Pending assignment |
| Owner decision | Pending |
| Decision date | Pending |
| Accepted ADRs | Pending |
| Deferred items and rationale | Pending |
| Remaining blockers | ARCH-01 through ARCH-07 |
| Outcome | Not accepted |

M1 acceptance approves a design. It does not itself execute changes or authorize an unspecified deployment. Next step after acceptance: define the bounded manual pilot and its change/rollback procedure.
