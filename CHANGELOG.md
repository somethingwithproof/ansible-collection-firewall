# Changelog

## Unreleased

- Preserve the public policy entry point and `firewall_*` variables through the
  canonical firewall role.
- Lock CI tooling, isolate public PR runners, and add selective Sonar analysis.

## 0.1.0

- Initial policy abstraction and nftables implementation with staged validation,
  backup, rollback, and an Ubuntu/Debian Molecule scenario.
