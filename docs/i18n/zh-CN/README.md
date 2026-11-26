<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · **简体中文** · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · **简体中文** · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

一个用于描述主机防火墙策略并应用 nftables 配置的 Ansible 集合。地址组将策略与生成的规则集分离。

## 实现与边界

- [firewall](../../../roles/firewall/tasks/main.yml) 负责公开的默认配置和后端验证；[policy](../../../roles/policy/tasks/main.yml) 通过导入前者保留旧版入口点。
- [nftables](../../../roles/nftables/tasks/main.yml) 负责安装 nftables、暂存配置文件、使用 `nft -c -f` 进行验证，并使用 `nft -f` 应用配置。
- **`nftables_backup: true`** — 如果存在先前的配置，应用代码块会在失败后尝试恢复该配置。备份可用性和恢复访问路径仍然是运维要求。
- [模板](../../../roles/nftables/templates/nftables.conf.j2) 和 [默认配置](../../../roles/firewall/defaults/main.yml) 定义了支持的策略字段。

本集合打包了 `firewall`、`policy` 和 `nftables`；仅 `nftables` 实现了防火墙后端。策略验证接受 iptables 和 firewalld 的后端名称，但此处未提供对应的后端角色。模板中的 SSH 规则并不能保证防止锁定。仅验证模式仍然会安装软件包并执行配置/服务任务；它并非无副作用的空运行。

## 快速开始

从当前检出构建并安装此集合：

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

文件名遵循 [galaxy.yml](../../../galaxy.yml) 中的版本号。如果版本号发生变化，请使用构建生成的文件名。

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
        office: ["203.0.113.0/24"]  # 替换为实际的办公网络 (CIDR)
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

在将生成的策略应用到远程主机之前，请先在测试主机上进行审查。更改防火墙策略时，请保留独立的恢复路径。

## 开发

[CI 工作流](../../../.github/workflows/ci.yml) 和 [Molecule 场景](../../../molecule) 描述了已配置的验证流程。它们的存在并不证明在所有发行版上都能成功部署。

## 合规与许可证

早期的 README 引用了一份合规性映射文档，但当前检出中不包含该文档。此处不声明任何认证或框架合规性。本集合使用 [MIT 许可证](../../../LICENSE)；[galaxy.yml](../../../galaxy.yml) 记录了软件包元数据。

## 操作系统支持

nftables 后端已在 Ubuntu 22.04、24.04 和 26.04 LTS、Debian 12 和 13 以及 Rocky Linux 9 和 10 上使用 Ansible core 2.21 进行测试。对于 Ubuntu 26.04 的 Python 3.14，请使用带有 core 2.20 或更高版本的受维护控制节点。

Molecule 会检查收敛性、无变更的第二次运行、已加载的规则集、SSH 22 端口允许规则、配置文件权限以及注入应用失败后的回滚。Enterprise Linux 使用 `/etc/sysconfig/nftables.conf`；Debian 和 Ubuntu 使用 `/etc/nftables.conf`。`nftables_conf_path` 仍然可配置。在使用此覆盖项之前，请先配置原生 nftables 服务以加载自定义路径：该角色会重启服务，否则服务会加载其默认文件并可能替换预期的规则集。

在 Enterprise Linux 上，如果存在内容不同的旧版 `/etc/nftables.conf`，角色将在应用生效前停止。请备份这两个文件，然后将已验证的先前配置移动到原生服务路径。或者，明确保留旧版路径并相应配置服务。不要在 `/etc/nftables.conf` 留下过时的副本：该处内容不同的文件会在后续运行时触发保护机制。升级期间，先前的规则仍保存在所选路径中作为回滚来源。

Rocky Linux 用于演练 Enterprise Linux 的软件包和服务路径。这些容器测试不对 RHEL 订阅或生产内核提供认证。未启用扩展支持的 Ubuntu 20.04、Debian 11 以及 EL8 较旧的 Python 版本均不在当前测试矩阵中。
