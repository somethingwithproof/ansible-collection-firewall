# thomasvincent.firewall

Enterprise-grade Ansible collection for host firewall management. Primary backend: nftables. Legacy support: iptables. Optional: firewalld wrapper. Includes safety (atomic apply, backups, SSH lockout guard), rollback, Molecule tests, and compliance mapping.

## Architecture

![Architecture Diagram](architecture.png)

## Requirements

- **Ansible version**: 2.9 or higher
- **OS Support**: Ubuntu 20.04+, Debian 11+, RHEL 8+, CentOS 8+
- **Python**: 3.6 or higher

## Collection Variables

Example inventory variables (group_vars/all.yml):

```yaml
firewall_backend: auto             # auto|nftables|iptables
firewall_defaults:
  policy_v4: drop
  policy_v6: drop
  allow_loopback: true
  allow_established: true
  drop_invalid: true
  log_drops: true
  ssh_guard: true                 # temporary guard to keep SSH open during apply
  ssh_ports: [22]

firewall_objects:
  services:
    http: { ports: [80], proto: tcp }
    https: { ports: [443], proto: tcp }
  address_groups:
    office: ["203.0.113.0/24"]
  port_groups:
    web: [80, 443]

firewall_rules:
  - name: allow-web-from-office
    family: inet
    chain: input
    src_groups: [office]
    services: [http, https]
    proto: tcp
    state: present
    comment: "Permit office to web"
```

## Dependencies

None.

## Usage

Install the collection:

```bash
ansible-galaxy collection install thomasvincent.firewall
```

Example playbook:

```yaml
- hosts: all
  become: true
  roles:
    - role: thomasvincent.firewall.policy
    - role: thomasvincent.firewall.nftables
```

## Backends

- **nftables**: renders complete config and loads atomically (`nft -f`)
- **iptables**: renders rules.v4/v6 and restores via `iptables-restore`/`ip6tables-restore`

## Safety and Rollback

- Pre-apply backup of current rules; restore on failure
- SSH guard to avoid lockouts (port(s) in `firewall_defaults.ssh_ports`)
- Validate-only mode: `firewall_validate_only: true`

## Development

Contributions are welcome! Please ensure all changes pass linting and tests.

```bash
# Run linting
yamllint .
ansible-lint

# Run tests
molecule test
```

## Testing

Molecule scenarios for Ubuntu/Debian/RHEL. CI runs ansible-lint, yamllint, and Molecule idempotence.

```bash
molecule test
```

## Compliance

See docs/compliance.md for mappings to CIS Linux, NIST SP 800-53, ISO 27001 Annex A/ISO 27002, PCI DSS 4.0, and SOC 2 CC series.

## License

MIT
