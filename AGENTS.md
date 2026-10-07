<!--
SPDX-FileCopyrightText: 2026 Thomas Vincent
SPDX-License-Identifier: MIT
-->

# Firewall collection agent instructions

## Project and boundaries

`galaxy.yml` defines the `thomasvincent.firewall` Ansible collection.
`roles/policy/` resolves backend facts; `roles/nftables/` renders, validates and
applies nftables configuration with a backup/rescue path. Defaults, handlers
and the Jinja template belong to those roles. `molecule/default/` defines the
Docker test environment, converge playbook and verifier.

Only the nftables role is shipped here. Backend names in policy validation or
README claims do not establish a working iptables/firewalld implementation.
Likewise, configured compliance mappings are not certification or acceptance.

## Checks and packaging

Select Python through `mise`, install tooling in an isolated environment, and
inspect `.github/workflows/ci.yml` for the CI dependency setup.

```sh
mise exec python@3.12 -- yamllint .
mise exec python@3.12 -- ansible-lint
mise exec python@3.12 -- ansible-galaxy collection build --output-path /tmp/firewall-artifacts
```

`galaxy.yml` uses list-valued `authors` and `license`; keep `build_ignore`
excluding local assistant state. Verify that built archives include both roles
and no credentials/session state. A successful archive build proves packaging,
not firewall correctness.

With an explicitly selected disposable Docker environment, run:

```sh
mise exec python@3.12 -- molecule test
```

The scenario uses privileged containers and modifies their firewall/services.
Never run it against the developer host or a production inventory.

## Firewall safety and review

Preserve syntax validation before promotion, backup permissions, atomic apply,
rollback behavior, IPv4/IPv6 rules and SSH access protections. Test failure and
lockout paths as well as convergence/idempotence. Do not widen default access to
make tests pass. `firewall_validate_only` still runs package/service tasks and
may notify handlers; it must not be described as a non-mutating offline check.
Keep variables under the existing `firewall_` and `nftables_` namespaces.

## Working rules

Select required language runtimes through `mise`. Read the checked-out manifests,
lockfiles and GitHub Actions before choosing versions or commands; do not infer
support from an old README example. Keep changes focused and preserve public
interfaces, licenses and existing correctness/security checks.

Keep credentials, customer data, `.omc/`, `.worktrees/` and generated output out
of commits. Never disable checks or suppress findings just to obtain a passing
result. Report the commands run, results and untested environments. Publishing,
deploying, modifying live systems and merging require task authorization.
