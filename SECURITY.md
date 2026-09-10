# Public repository security boundary

**ACCEPTED CONSTRAINT:** the repository must never contain real environment data. This applies to content and Git history, not just the latest working tree.

## Allowed public material

Reusable design, synthetic diagrams, templates, generalized failure narratives and examples under `data/example/`. Use `corp.example.test` for illustrative identity/DNS names and `10.250.x.0/24` for illustrative IPv4 networks. Omit real device identities and addresses rather than publishing a reversible mapping. Generic product models, supplied versions, structural VLAN IDs and bridge names describe architecture without identifying devices.

## Prohibited material

Real HomeLab IP addresses, internal subnets, real domains, public IPs, MAC addresses, serial numbers, usernames, passwords, tokens, API keys, VPN endpoints, tenant/account identifiers, device-specific hostnames and any other environment-specific data. Also exclude raw exports, inventory, screenshots, diagnostic bundles, PCAPs, state, plans, backups, private certificates/keys, logs and hidden metadata that can carry these values. Encrypted secrets and encrypted device backups also stay outside public Git.

## External private boundary

Keep the actual inventory, address plan, real-to-example mapping, secrets, Terraform state/plans, backups and recovery evidence outside the checkout and outside public CI artifacts. The private store, access model, retention and backup mechanism remain an OPEN DECISION. No private path, server name or secret reference is committed in this skeleton.

Future tools will consume protected runtime inputs using a reviewed interface. Do not create a live inventory or deployable default from `data/example/`. CI logs, saved plans and approval artifacts must be handled according to their sensitivity.

## Before every publication

1. Inspect staged filenames and the complete diff; inspect new binary files and archive contents separately. Prefer synthetic text evidence to screenshots.
2. Search for non-example addresses/domains, environment identifiers and credentials; use secret detection when selected. Do not embed a real-value denylist in public Git.
3. Inspect commit author/email, branch names, remote URLs, issue/PR text, filenames and artifact metadata for environment information. Use a deliberate public commit identity.
4. Verify no state, plans, private inventory, exports or backups are tracked. `.gitignore` does not protect already tracked files, forced additions, Git history or issue text.
5. Review rendered docs as well as source. Preserve a sanitized evidence narrative without publishing raw evidence.

Scanner selection and validation workflow are future work; no automated safety guarantee is claimed. Branch protections and repository settings must later be configured on the chosen host; templates do not enforce them.

## Future CI trust rules

Untrusted contributions may run only isolated validation on synthetic data. Never expose self-hosted infrastructure runners or private secrets to fork/PR code. Planning against real infrastructure belongs in a trusted private context. Approvals must bind the exact revision and plan; changes invalidate approval. Select scoped identities, secret delivery, protected environments and retention during the automation design review.

## Accidental disclosure

Stop publication and affected pipelines. If a credential was exposed, revoke/rotate it and assess access. Remove sensitive data from every affected history/artifact/log surface using an approved incident procedure; deleting a working-tree file is insufficient. Inspect forks/caches where possible. Publish only a sanitized incident summary. Do not file sensitive details in public issues; a private contact route is an OPEN DECISION before publication.
