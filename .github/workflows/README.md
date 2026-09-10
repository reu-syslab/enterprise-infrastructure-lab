# CI workflow responsibility reservation

No executable workflow exists. This directory deliberately contains documentation only because automation implementation is deferred.

Future stages: public synthetic validation → trusted private planning → approval bound to revision/plan → private/self-hosted execution → verification/monitoring. Decide workflow triggers, pinned dependencies, token permissions, protected environments, state/plan retention, concurrency and secret delivery during architecture review.

Untrusted PR code must never run on a privileged self-hosted runner or gain private secrets/network access. Real state, plans, inventories and logs must not become public artifacts. Branch protections and approval settings need actual host configuration later; this README does not enforce them.
