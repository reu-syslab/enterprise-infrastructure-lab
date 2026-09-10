# Firewall intent and review matrix

**PROPOSED TARGET STATE — documentation only; no deployable rules.** A live read-only FortiGate audit confirmed the existence and broad intent of current inter-VLAN, egress and WAN NAT/VIP policies. Raw policy order, real objects and endpoints remain private. This file defines the future reviewed intent and does not reproduce installed permissions.

All rows require exact protocol/port, destination identity, direction, policy ordering, logging, NAT implications and verification review before implementation. “Required service subset” is unresolved and must never become an allow-any rule. Return traffic/state behavior must be verified in the approved design.

| ID | Initiating source role | Destination role | Purpose | Protocol/port | Intent | Status | Verification requirement |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FW-01 | WAN | New enterprise AD services | Prevent public exposure | All; inspect VIP/NAT paths too | No published access | ACCEPTED CONSTRAINT; enforcement design open | External reachability denied; no exposing VIP |
| FW-02 | WAN-exposed DMZ/test/game workloads | Enterprise identity environment | Isolate exposed workloads | All unsolicited initiation | Deny | ACCEPTED CONSTRAINT; exact scope/open flows reviewed | Denied reachability from every relevant exposed segment |
| FW-03 | Approved management origin | Selected network/virtualization/OS management endpoints | Administration and recovery | OPEN DECISION | Minimal approved access | PROPOSED TARGET STATE | Allowed from approved origin; denied from other roles |
| FW-04 | Domain Windows client | Domain services | Join, sign-in, policy and domain discovery | Required service subset — OPEN DECISION | Allow reviewed subset | PROPOSED TARGET STATE | Join/sign-in/policy succeed; unrelated access denied |
| FW-05 | Domain-integrated Linux host | Domain services | Kerberos/SSSD identity and authorization | Required service subset — OPEN DECISION | Allow reviewed subset | PROPOSED TARGET STATE | Authorized login works; denied identity remains denied |
| FW-06 | Approved DNS consumer | Assigned resolver | Name resolution | DNS transport details — OPEN DECISION | Allow reviewed subset | PROPOSED TARGET STATE | Authoritative/internal and external resolution; other recursion denied |
| FW-07 | AD DNS / BIND9 / Unbound resolver role | Reviewed authority or forwarder role | DNS coexistence | OPEN DECISION; direction not selected | Pending DNS design | OPEN DECISION | No forwarding loop; correct authority and outage behavior |
| FW-08 | Authorized client role | Windows File Server | File access | SMB service subset — OPEN DECISION | Allow reviewed subset | PROPOSED TARGET STATE | SMB reachability plus separate share/NTFS positive/negative tests |
| FW-09 | Zabbix or monitored endpoint | Counterpart monitoring endpoint | Health collection | Method and initiation direction — OPEN DECISION | Pending monitoring design | OPEN DECISION | Collection works; non-monitoring access restricted |
| FW-10 | Approved enterprise workloads | Approved update/time/external services | Updates and time dependencies | Destination/service subset — OPEN DECISION | Restricted egress proposal | PROPOSED TARGET STATE | Required service succeeds; unapproved egress fails |
| FW-11 | Enterprise role | Other enterprise/legacy role | Catch unspecified lateral paths | All otherwise unmatched | Default-deny proposal | PROPOSED TARGET STATE | Representative unapproved cross-role attempts fail |

## Review requirements

Record exact service dependencies after namespace/topology and product choices are reviewed. Include DNS and time dependencies in AD/Linux flows; do not guess a sufficient AD port list. Decide zone usage rather than assuming zones already exist. Check broad pre-existing rules, overlapping objects, same-segment traffic paths and NAT/VIP behavior; inter-VLAN firewall rules alone do not establish isolation within a shared segment.

File authorization is separately governed by SMB share and NTFS permissions; network access does not grant data access. Document rule IDs/roles abstractly in public evidence, with actual object names/addresses kept external. No production-style scan or negative test is authorized by this matrix.
