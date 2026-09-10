# Terraform responsibility reservation

Future scope: reviewed declarative infrastructure lifecycle with protected state and environment inputs. `proxmox/` and `fortigate/` document separate candidate ownership domains. Provider selection, versions, backend, import strategy, module structure and object ownership remain OPEN DECISION.

This directory contains no provider blocks, resources, modules, variables or state configuration. Do not run initialization, plan or apply for this skeleton. Provider lockfiles should be committed only after an approved provider selection; state and real plans must remain private.
