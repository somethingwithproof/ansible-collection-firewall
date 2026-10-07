<!--
SPDX-FileCopyrightText: 2026 Thomas Vincent
SPDX-License-Identifier: MIT
-->

# ansible-collection-firewall implementation and review instructions

Read [AGENTS.md](../AGENTS.md) for the complete repository-specific guidance.

## Review priorities

Review staged ruleset validation, backup/rollback correctness, SSH continuity and IPv4/IPv6 behavior.

Require focused regression evidence for changed behavior and preserve existing
correctness/security checks. YAML/Ansible lint and an offline collection build; Molecule only in a disposable Docker environment.

Flag unsupported maturity claims, hidden failures, credentials in code/logs, and
generated output presented as first-party implementation. Review permissions,
immutable Action pins and fork-secret isolation when workflows change. A skipped
check or absent check result is not proof that validation ran.

Keep changes within the requested scope and use `mise` for language runtimes.
