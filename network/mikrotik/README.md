# MikroTik engineering boundary

Own future RouterOS manual records, reusable configuration design, bootstrap/adoption/recovery and validation. Confirmed RB5009 role: downstream of FortiGate over an 802.1Q trunk, connecting the physical MGMT workstation and Proxmox trunk. Do not infer RouterOS version, port IDs, management IP or exact bridge/PVID settings.

Tooling and API/automation method are OPEN DECISION. Review safe VLAN filtering/tagging changes, management lockout prevention, protected interface mapping, firmware compatibility, restore order and same-model replacement verification. Routing ownership remains the confirmed FortiGate baseline unless a later architecture decision changes it.

No RouterOS exports, `.rsc` scripts or configuration templates are included. Environment exports and backups stay external.
