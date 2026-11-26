<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · **日本語** · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · **日本語** · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

ホストのファイアウォールポリシーを記述し、nftables 設定を適用するための Ansible コレクションです。アドレスグループにより、ポリシーと生成されるルールセットを分離して管理できます。

## 実装と適用範囲

- [firewall](../../../roles/firewall/tasks/main.yml) は公開デフォルト値とバックエンドの検証を担い、[policy](../../../roles/policy/tasks/main.yml) はこれをインポートすることで従来のエントリーポイントを維持します。
- [nftables](../../../roles/nftables/tasks/main.yml) は nftables のインストール、設定のステージング、`nft -c -f` による検証、および `nft -f` による適用を行います。
- **`nftables_backup: true`** — 適用ブロックは、以前の有効な設定が存在していた場合、失敗時にその設定への復元を試みます。バックアップの確保と復旧用のアクセス経路の維持は、引き続き運用上の要件となります。
- サポートされるポリシーフィールドは、[テンプレート](../../../roles/nftables/templates/nftables.conf.j2) と [デフォルト設定](../../../roles/firewall/defaults/main.yml) で定義されています。

このコレクションには `firewall`、`policy`、`nftables` が含まれていますが、ファイアウォールバックエンドを実装しているのは `nftables` のみです。ポリシー検証では iptables および firewalld のバックエンド名も受け付けますが、対応するバックエンドロールは本コレクションには含まれていません。テンプレート内の SSH ルールはロックアウトを防ぐ保証にはなりません。検証専用モードでもパッケージのインストールや設定・サービスタスクは実行されるため、副作用のないドライランではありません。

## クイックスタート

このチェックアウトからコレクションをビルドしてインストールします。

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

ファイル名は [galaxy.yml](../../../galaxy.yml) のバージョンに従います。バージョンが変更された場合は、ビルドによって生成されたファイル名を使用してください。

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
        office: ["203.0.113.0/24"]  # 実際のオフィスのネットワークに置き換えてください (CIDR)
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

リモートホストに適用する前に、テストホスト上で生成されたポリシーを確認してください。ファイアウォールポリシーを変更する際は、独立した復旧経路を確保しておいてください。

## 開発

設定されている検証内容は [CI ワークフロー](../../../.github/workflows/ci.yml) および [Molecule シナリオ](../../../molecule) に記述されています。これらが存在するからといって、すべてのディストリビューションでのデプロイ成功が保証されるわけではありません。

## コンプライアンスとライセンス

以前の README で参照されていたコンプライアンスマッピング文書は、このチェックアウトには含まれていません。ここではいかなる認証やフレームワークへの準拠も主張していません。このコレクションは [MIT ライセンス](../../../LICENSE) の下で提供され、パッケージのメタデータは [galaxy.yml](../../../galaxy.yml) に記載されています。

## サポート対象 OS

nftables バックエンドは、Ubuntu 22.04、24.04、26.04 LTS、Debian 12 および 13、Rocky Linux 9 および 10 上で Ansible core 2.21 を用いてテストされています。Ubuntu 26.04 の Python 3.14 に対応するには、保守されている Ansible core 2.20 以降のコントローラーを使用してください。

Molecule では、収束、変更が発生しない 2 回目の実行、ロードされたルールセット、SSH ポート 22 の許可ルール、設定ファイルのパーミッション、および適用失敗を注入した際のロールバックを検証しています。Enterprise Linux では `/etc/sysconfig/nftables.conf` を、Debian および Ubuntu では `/etc/nftables.conf` を使用します。`nftables_conf_path` は引き続き設定可能です。この上書きを使用する前に、カスタムパスを読み込むようにネイティブの nftables サービスを設定してください。ロールはサービスを再起動するため、設定していない場合は標準ファイルが読み込まれ、意図したルールセットが上書きされる可能性があります。

Enterprise Linux では、内容の異なる従来の `/etc/nftables.conf` が存在する場合、設定の昇格前にロールの実行が停止します。両方のファイルをバックアップしたうえで、検証済みの以前の設定をネイティブのサービスパスに移動してください。あるいは、従来のパスを明示的に維持し、それに合わせてサービスを設定してください。`/etc/nftables.conf` に古いコピーを残さないでください。内容が異なるファイルがあると、以降の実行でガードが作動します。選択したパスにある以前のルールは、アップグレード時のロールバック元として引き続き利用できます。

Rocky Linux により、Enterprise Linux のパッケージおよびサービス経路が検証されます。これらのコンテナテストは、RHEL サブスクリプションや本番カーネルの動作を認定するものではありません。延長サポートなしの Ubuntu 20.04、Debian 11、および古い Python を使用する EL8 は、現在のテストマトリクスの対象外です。
