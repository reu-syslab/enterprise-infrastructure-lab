# FortiGate engineering boundary

Own future FortiGate manual configuration records, vendor-specific bootstrap/adoption/recovery design and verification. Baseline: FortiGate 80F, FortiOS 7.6.2, standalone NAT mode, no system zones; routing, DHCP, firewalling, WAN NAT/VIPs.

Plan safe management bootstrap, compatible firmware, protected inputs, restore order, trunk/interface and policy verification, and handoff to the accepted steady-state owner. Coordinate object ownership with `terraform/fortigate/`; avoid duplicate controllers. Zone introduction and all new policies/VLANs require review.

Equivalent same-model replacement recovery is a future acceptance objective. Raw exports, backups, serials, MACs and credentials must never be placed here. No vendor commands or templates are implemented.
