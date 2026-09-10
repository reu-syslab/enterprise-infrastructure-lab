# VLAN and IP planning register

**All subnets in this file are synthetic examples, not actual allocations.** VLAN IDs/roles in the current-state table are confirmed structural facts. Candidate IDs in the target table are unapproved. No host addresses, gateways, DHCP pools or real-to-example mapping are supplied.

## CONFIRMED CURRENT STATE — roles only; addressing sanitized

| VLAN | Confirmed purpose | Synthetic example subnet | Confirmed notes |
| --- | --- | --- | --- |
| 10 | MGMT | `10.250.10.0/24` | Physical MGMT workstation |
| 20 | SRV/Proxmox | `10.250.20.0/24` | Proxmox management and dedicated Twingate connector |
| 30 | LAB | `10.250.30.0/24` | Existing BIND9 and Unbound |
| 40 | Wi-Fi | `10.250.40.0/24` | Existing role |
| 50 | DMZ/Game | `10.250.50.0/24` | Existing role; do not infer all exposed test services reside here |
| 40 — alternate legacy configuration | Abandoned home-automation experiment | `10.250.140.0/24` | Alternate configuration of VLAN40; **not VLAN140**; FortiGate trunk binding confirmed, active consumers/dependencies unknown; preserve |

FortiGate provides existing VLAN gateways, DHCP, routing and firewalling. Gateway host numbers and pool limits are intentionally unspecified.

## PROPOSED TARGET STATE — requires architecture review

| Candidate VLAN | Candidate purpose | Synthetic example only | Review issue |
| --- | --- | --- | --- |
| 60 | Enterprise Servers | `10.250.60.0/24` | Server/identity trust boundaries and capacity |
| 70 | Windows Clients | `10.250.70.0/24` | Pilot clients and later physical-client access |
| 80 | Linux | `10.250.80.0/24` | Domain integration and service isolation |
| 90 | Monitoring/Test | `10.250.90.0/24` | Whether monitoring and test workloads should be separated |

## OPEN DECISION — allocation checklist

Review IDs for conflicts, subnet sizing/overlap (including remote access), gateway/DHCP ownership, pools/reservations, DNS options, required trunk propagation, access-port membership, VM tags, IPv6 policy, routing and management restrictions. Keep the actual plan external; publish only abstract roles and synthetic examples. Unlisted infrastructure is not assumed to exist.

Do not change legacy attachments, create candidate VLANs or apply this example table to hardware. Track acceptance in ARCH-02 and an ADR.
