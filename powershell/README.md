# PowerShell responsibility reservation

Future scope: Windows administration and verification for AD, AD-integrated DNS, GPO, client membership, Windows File Server, SMB/NTFS permissions and troubleshooting. Script names, modules, supported versions and execution privileges remain undecided.

Scripts must eventually declare inputs, prerequisites, expected effects, repeatability, failure handling, output redaction and rollback. Review invocation/ownership boundaries with Ansible. Keep real domains, identities and secrets out of scripts and examples.

No scripts or DC promotion/domain join commands are implemented in this skeleton.
