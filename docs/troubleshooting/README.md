# Troubleshooting scenario responsibility

Own controlled failure narratives that connect symptoms to hypotheses, evidence, root cause, recovery and prevention. Use [scenario-template.md](scenario-template.md). Scenarios are planned learning coverage, not authorization to inject failures.

Future candidates: incorrect VM VLAN tag; broken DNS forwarding or unavailable resolver; domain discovery/time dependency failure; GPO scope error; SMB versus NTFS permission denial; Linux Kerberos/SSSD authorization failure; missing monitoring collection; and loss of management connectivity. Review isolation, blast radius and recovery before any exercise. Do not run disruptive tests against shared or WAN-exposed services without a separately defined scope.
