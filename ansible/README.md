# Ansible responsibility reservation

Future scope: reusable OS/service orchestration, with roles under `roles/`. Inventory, credentials and actual hosts stay outside the checkout. Connection transports, Windows bootstrap, collection/version selection, role interfaces and execution model require architecture review.

No playbooks, inventory, collections or configuration are implemented. Candidate Windows actions must have a single owner shared clearly with `powershell/`; Linux/identity/DNS changes require the accepted service design first.
