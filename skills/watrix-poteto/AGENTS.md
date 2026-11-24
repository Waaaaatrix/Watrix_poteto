# Playbook instructions

Load this file before selecting or running any playbook in this collection, including a directly named playbook.

## Testing

Only an explicit user invocation of `\test` in the current request authorizes testing within the requested scope. Route that work to [Test](test/playbooks/test.md).

Without `\test`, do not write or modify tests, run test commands, perform manual reproductions or smoke tests, run benchmarks, start or retry CI tests, or delegate any of those actions. This rule applies even when a playbook or skill calls for them.
