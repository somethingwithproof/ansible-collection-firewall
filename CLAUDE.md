<!--
SPDX-FileCopyrightText: 2026 Thomas Vincent
SPDX-License-Identifier: MIT
-->

# ansible-collection-firewall coding guide

Read [AGENTS.md](AGENTS.md) before making changes. It contains the repository's
architecture, canonical commands, supported boundaries and operating rules.
Follow any applicable nested instructions and the checked-out CI configuration.

## Project priorities

Review staged ruleset validation, backup/rollback correctness, SSH continuity and IPv4/IPv6 behavior.

## Verification

YAML/Ansible lint and an offline collection build; Molecule only in a disposable Docker environment.

Separate offline checks from operations that change hosts, databases, firewalls
or published artifacts. State verification limits and preserve existing controls.
Select language runtimes through `mise`; never commit local session state or secrets.
