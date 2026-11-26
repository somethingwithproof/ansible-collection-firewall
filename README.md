<!-- translations:start -->
**Languages:** **English** · [Deutsch](docs/i18n/de/README.md) · [Español](docs/i18n/es/README.md) · [Français](docs/i18n/fr/README.md) · [Português (BR)](docs/i18n/pt-BR/README.md) · [Русский](docs/i18n/ru/README.md) · [日本語](docs/i18n/ja/README.md) · [한국어](docs/i18n/ko/README.md) · [简体中文](docs/i18n/zh-CN/README.md) · [العربية](docs/i18n/ar/README.md) · [हिन्दी](docs/i18n/hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** **English** · [Deutsch](docs/i18n/de/README.md) · [Español](docs/i18n/es/README.md) · [Français](docs/i18n/fr/README.md) · [Italiano](docs/i18n/it/README.md) · [Português (BR)](docs/i18n/pt-BR/README.md) · [Nederlands](docs/i18n/nl/README.md) · [Русский](docs/i18n/ru/README.md) · [Українська](docs/i18n/uk/README.md) · [日本語](docs/i18n/ja/README.md) · [한국어](docs/i18n/ko/README.md) · [简体中文](docs/i18n/zh-CN/README.md) · [繁體中文](docs/i18n/zh-TW/README.md) · [العربية](docs/i18n/ar/README.md) · [हिन्दी](docs/i18n/hi/README.md) · [Polski](docs/i18n/pl/README.md) · [Türkçe](docs/i18n/tr/README.md) · [Svenska](docs/i18n/sv/README.md) · [Tiếng Việt](docs/i18n/vi/README.md) · [Bahasa Indonesia](docs/i18n/id/README.md)
- **Africa:** [Afrikaans](docs/i18n/af-ZA/README.md) · [አማርኛ](docs/i18n/am-ET/README.md) · [العربية (مصر)](docs/i18n/ar-EG/README.md) · [العربية (المغرب)](docs/i18n/ar-MA/README.md) · [Français (Maroc)](docs/i18n/fr-MA/README.md) · [Français (Sénégal)](docs/i18n/fr-SN/README.md) · [Hausa](docs/i18n/ha-NG/README.md) · [Igbo](docs/i18n/ig-NG/README.md) · [Malagasy](docs/i18n/mg-MG/README.md) · [Kinyarwanda](docs/i18n/rw-RW/README.md) · [Soomaali](docs/i18n/so-SO/README.md) · [Kiswahili](docs/i18n/sw-KE/README.md) · [isiXhosa](docs/i18n/xh-ZA/README.md) · [Yorùbá](docs/i18n/yo-NG/README.md) · [isiZulu](docs/i18n/zu-ZA/README.md)
- **Asia:** [বাংলা](docs/i18n/bn/README.md) · [فارسی](docs/i18n/fa/README.md) · [עברית](docs/i18n/he/README.md) · [मराठी](docs/i18n/mr/README.md) · [தமிழ்](docs/i18n/ta/README.md) · [తెలుగు](docs/i18n/te/README.md) · [ไทย](docs/i18n/th/README.md) · [اردو](docs/i18n/ur/README.md) · [Bahasa Melayu](docs/i18n/ms/README.md) · [Filipino](docs/i18n/fil/README.md)

</details>
<!-- translations:end -->

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
before promotion. Back up both files, then move the validated previous
configuration to the native service path. Alternatively, explicitly retain
the legacy path and configure the service accordingly. Do not leave a stale
copy at `/etc/nftables.conf`: a differing file there triggers the guard on
later runs. The previous rules remain available at the selected path as the
rollback source during an upgrade.

Rocky Linux exercises the Enterprise Linux package and service path. These
container tests do not certify RHEL subscriptions or a production kernel.
Ubuntu 20.04 without extended support, Debian 11 and EL8's older Python are
outside the current test matrix.
