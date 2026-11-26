<!-- translations:start -->
**Languages:** [English](../../../README.md) · **繁體中文** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · **繁體中文** · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

用於描述主機防火牆政策並套用 nftables 設定的 Ansible collection。透過位址群組將政策與產生的規則集分離。

## 實作與邊界

- [firewall](../../../roles/firewall/tasks/main.yml) 負責公開預設值與後端驗證；[policy](../../../roles/policy/tasks/main.yml) 透過匯入該角色來保留舊版進入點。
- [nftables](../../../roles/nftables/tasks/main.yml) 負責安裝 nftables、暫存設定檔、使用 `nft -c -f` 進行驗證，並使用 `nft -f` 套用設定。
- **`nftables_backup: true`** — 若先前已存在設定檔，套用區塊會在失敗後嘗試還原舊有設定。備份可用性與復原存取路徑仍屬於維運需求。
- [範本](../../../roles/nftables/templates/nftables.conf.j2)與[預設值](../../../roles/firewall/defaults/main.yml)定義了支援的政策欄位。

本 collection 封裝了 `firewall`、`policy` 與 `nftables`；其中僅 `nftables` 實作了防火牆後端。政策驗證雖然接受 iptables 與 firewalld 的後端名稱，但此處並未隨附對應的後端角色。範本中的 SSH 規則不保證能避免遭到鎖定。僅驗證模式（validate-only mode）仍會安裝套件並執行設定／服務工作，並非無副作用的模擬執行（dry run）。

## 快速開始

從此程式碼檢出建置並安裝 collection：

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

檔案名稱遵循 [galaxy.yml](../../../galaxy.yml) 中的版本號。若該版本號變更，請改用建置所產生的檔案名稱。

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
        office: ["203.0.113.0/24"]  # 請替換為實際的辦公網路 (CIDR)
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

套用至遠端主機前，請先於測試主機檢視產生的政策。變更防火牆政策時，請保留獨立的復原存取路徑。

## 開發

[CI 工作流程](../../../.github/workflows/ci.yml)與 [Molecule 情境](../../../molecule)描述了已設定的驗證方式。這些項目的存在並不代表能在每個發行版本上成功部署。

## 合規與授權

早期的 README 曾提及一份合規對應文件，但該文件並未存在於目前的檢出版本中。本專案不宣稱符合任何認證或框架合規要求。本 collection 採用 [MIT 授權條款](../../../LICENSE)；[galaxy.yml](../../../galaxy.yml) 記錄了套件中繼資料。

## 作業系統支援

nftables 後端已使用 Ansible core 2.21 在 Ubuntu 22.04、24.04 與 26.04 LTS、Debian 12 與 13，以及 Rocky Linux 9 與 10 上完成測試。針對 Ubuntu 26.04 的 Python 3.14，請使用受維護且搭載 core 2.20 或更新版本的控制器。

Molecule 會檢查收斂性（convergence）、第二次執行無變更、已載入的規則集、SSH 連接埠 22 允許規則、設定檔權限，以及在注入套用失敗後的還原行為。Enterprise Linux 使用 `/etc/sysconfig/nftables.conf`；Debian 與 Ubuntu 使用 `/etc/nftables.conf`。`nftables_conf_path` 仍可自行設定。使用該覆寫設定前，請先設定原生的 nftables 服務以載入任何自訂路徑：角色會重新啟動該服務，否則服務會載入其預設檔案，進而可能取代預期的規則集。

在 Enterprise Linux 上，若存在內容不同的舊版 `/etc/nftables.conf`，角色會在正式切換前中止執行。請先備份兩個檔案，再將已驗證的舊有設定移至原生服務路徑；或者明確保留舊版路徑並據此設定服務。請勿在 `/etc/nftables.conf` 留下過時的複本：該處若存在內容不同的檔案，後續執行時將觸發保護機制。升級期間，先前的規則仍會保留於所選路徑中，作為還原來源。

Rocky Linux 用於驗證 Enterprise Linux 的套件與服務路徑。這些容器測試不代表對 RHEL 訂閱或正式環境核心的認證。未啟用延伸支援的 Ubuntu 20.04、Debian 11，以及 EL8 較舊的 Python 版本均不在目前的測試矩陣範圍內。
