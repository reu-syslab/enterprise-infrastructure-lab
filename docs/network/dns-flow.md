# DNS flow and coexistence design

**CONFIRMED CURRENT STATE:** BIND9 and Unbound exist in VLAN30. FortiGate itself uses BIND9 as its configured resolver. DHCP on selected existing segments advertises BIND9 directly, while Wi-Fi and DMZ/Game use FortiGate default DNS service. Zone authority, BIND9/Unbound forwarding/recursion relationships, ACLs, complete consumer mapping and redundancy behavior are not yet confirmed.

**ACCEPTED CONSTRAINT:** plan AD-integrated DNS before changing existing DNS services or consumers. `corp.example.test` is synthetic and is not an accepted real domain name.

## Current discovery questions

Privately identify the resolver advertised to each client class; manually configured consumers; authoritative zones and reverse zones; forwarding/recursion relationships; recursion ACLs; DHCP DNS options; and the administration paths dependent on each resolver. Publish sanitized findings only. Do not infer BIND9 → Unbound or Unbound → BIND9 from their existence in the same segment.

## PROPOSED TARGET STATE — logical requirements

| Query class | Intended outcome | Unresolved routing |
| --- | --- | --- |
| Domain-member queries for the AD namespace/service records | Reach the chosen AD-integrated authority through the approved client DNS design | Direct client resolver selection, server count, DHCP distribution and failure behavior |
| Domain-member queries for external names | Resolve through approved recursion/forwarding without breaking domain discovery | Whether AD DNS uses Unbound, BIND9 or another reviewed upstream role |
| Existing consumers querying the new AD namespace, if needed | Reach AD authority only for an approved need | Whether conditional forwarding/delegation is required and on which resolver |
| Existing non-AD internal zones | Preserve existing authority and consumers through the pilot | Zone ownership, forwarding and any later migration |
| Reverse lookups | Match the reviewed address and authority plan | Reverse-zone ownership and update policy |

No specific forwarding chain has been selected. Never configure a loop between AD DNS, BIND9 and Unbound. Client fallback must preserve domain resolution; alternate DNS behavior is a design item, not an assumption that a second arbitrary resolver provides redundancy.

## OPEN DECISION — required choices

AD namespace and topology; authoritative boundaries; secure dynamic update policy; recursion access; forwarding versus delegation; reverse zones; caching/failure behavior; DHCP DNS options by segment; time-service dependencies; existing consumer migration; and monitoring/verification requirements. Record the decision in [ADR 0003](../adr/0003-dns-coexistence-proposed.md).

## Future verification and rollback contract

Before an authorized change, define private baseline capture and expected answers. Verify domain service-record discovery, authoritative answers, external queries, existing internal zones, reverse lookup behavior, controlled dynamic updates and denied unauthorized recursion. Test absence of forwarding loops and resolver-loss behavior, then verify domain sign-in/policy and Linux identity dependencies when those components exist.

Define abort criteria, restore points for resolver/DHCP settings, treatment of leases/caches and success checks for restored legacy consumers. These are design requirements; no DNS change or test has been executed.
