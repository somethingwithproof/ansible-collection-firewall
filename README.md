# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/license-MIT-blue)](./LICENSE)

An Ansible collection for describing host firewall policy and applying nftables configuration. Named services and address groups keep policy separate from the generated ruleset.

## Implementation and boundaries

- [firewall](roles/firewall/tasks/main.yml) owns public defaults and backend validation; [policy](roles/policy/tasks/main.yml) preserves the legacy entry point by importing it.
- [nftables](roles/nftables/tasks/main.yml) installs nftables, stages configuration, validates it with `nft -c -f`, and applies it with `nft -f`.
- The apply block attempts to restore a previous configuration after a failure when one existed. Backup availability and recovery access remain operational requirements.
- The [template](roles/nftables/templates/nftables.conf.j2) and [defaults](roles/policy/defaults/main.yml) define supported policy fields.

The collection packages `firewall`, `policy`, and `nftables`; only `nftables` implements a firewall backend. Backend names for iptables and firewalld are accepted by policy validation, but no corresponding backend roles are shipped here. An SSH rule in the template is not a guarantee against lockout. Validate-only mode still installs packages and performs configuration/service tasks; it is not a side-effect-free dry run.

## Quick start

Build and install the collection from this checkout:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

The filename follows the version in [galaxy.yml](galaxy.yml). Use the filename produced by the build if that version changes.

```yaml
- hosts: firewall_test_hosts
  become: true
  vars:
    firewall_backend: nftables
    firewall_defaults:
      policy_v4: drop
      policy_v6: drop
      allow_loopback: true
      allow_established: true
      drop_invalid: true
      log_drops: true
      ssh_guard: true
      ssh_ports: [22]
    firewall_objects:
      services:
        https: {ports: [443], proto: tcp}
      address_groups:
        office: ["203.0.113.0/24"]
      port_groups: {}
    firewall_rules:
      - name: office-https
        family: inet
        chain: input
        src_groups: [office]
        services: [https]
        proto: tcp
        state: present
  roles:
    - thomasvincent.firewall.policy
    - thomasvincent.firewall.nftables
```

Review the generated policy on a test host before applying it to remote hosts. Keep an independent recovery path when changing firewall policy.

## Development

The [CI workflow](.github/workflows/ci.yml) and [Molecule scenarios](molecule) describe the configured validation. Their presence does not establish successful deployment on every distribution.

## Compliance and license

The earlier README referenced a compliance-mapping document that is not present in this checkout. No certification or framework compliance is claimed here. The collection uses the [MIT license](LICENSE); [galaxy.yml](galaxy.yml) records the package metadata.
