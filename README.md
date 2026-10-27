# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](./LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

An Ansible collection for describing host firewall policy and applying nftables configuration. Address groups keep policy separate from the generated ruleset.

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
      address_groups:
        office: ["203.0.113.0/24"]  # TEST-NET-3 placeholder; replace with your office CIDR
      port_groups: {}
    firewall_rules:
      - name: office-https
        direction: inbound
        source_group: office
        dest_port: 443
        action: accept
  roles:
    - thomasvincent.firewall.policy
    - thomasvincent.firewall.nftables
```

Review the generated policy on a test host before applying it to remote hosts. Keep an independent recovery path when changing firewall policy.

## Development

The [CI workflow](.github/workflows/ci.yml) and [Molecule scenarios](molecule) describe the configured validation. Their presence does not establish successful deployment on every distribution.

## Compliance and license

The earlier README referenced a compliance-mapping document that is not present in this checkout. No certification or framework compliance is claimed here. The collection uses the [MIT license](LICENSE); [galaxy.yml](galaxy.yml) records the package metadata.

## Operating-system support

The nftables backend is tested with Ansible core 2.21 on Ubuntu 22.04,
24.04 and 26.04 LTS, Debian 12 and 13, and Rocky Linux 9 and 10. Use a
maintained controller with core 2.20 or newer for Ubuntu 26.04's Python 3.14.

Molecule checks convergence, a second run with no changes, the loaded ruleset,
an SSH port 22 allow rule, configuration permissions and rollback after an injected apply
failure. Enterprise Linux uses `/etc/sysconfig/nftables.conf`; Debian and
Ubuntu use `/etc/nftables.conf`. `nftables_conf_path` remains configurable.
Configure the native nftables service to load any custom path before using
that override: the role restarts the service, which otherwise loads its stock
file and can replace the intended ruleset.

On Enterprise Linux, a differing legacy `/etc/nftables.conf` stops the role
before promotion. Copy the validated previous configuration to the native
service path first, or explicitly retain the legacy path and configure the
service accordingly. This keeps the previous rules available as the rollback
source during an upgrade.

Rocky Linux exercises the Enterprise Linux package and service path. These
container tests do not certify RHEL subscriptions or a production kernel.
Ubuntu 20.04 without extended support, Debian 11 and EL8's older Python are
outside the current test matrix.
