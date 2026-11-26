<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Svenska** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · **Svenska** · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

En Ansible-samling för att beskriva värdbrandväggspolicy och tillämpa nftables-konfiguration. Adressgrupper håller policyn separerad från den genererade regeluppsättningen.

## Implementering och avgränsningar

- [firewall](../../../roles/firewall/tasks/main.yml) äger publika standardvärden och backend-validering; [policy](../../../roles/policy/tasks/main.yml) bevarar den äldre startpunkten genom att importera den.
- [nftables](../../../roles/nftables/tasks/main.yml) installerar nftables, förbereder konfigurationen, validerar den med `nft -c -f` och tillämpar den med `nft -f`.
- **`nftables_backup: true`** — Tillämpningsblocket försöker återställa en tidigare konfiguration efter ett fel när en sådan finns. Tillgång till säkerhetskopior och återställningsåtkomst förblir driftskrav.
- [Mallen](../../../roles/nftables/templates/nftables.conf.j2) och [standardvärdena](../../../roles/firewall/defaults/main.yml) definierar policyfält som stöds.

Samlingen paketerar `firewall`, `policy` och `nftables`; endast `nftables` implementerar en brandväggsbackend. Backend-namn för iptables och firewalld accepteras av policyvalideringen, men inga motsvarande backend-roller levereras här. En SSH-regel i mallen är ingen garanti mot utelåsning. Läget för enbart validering installerar fortfarande paket och utför konfigurations- och tjänstuppgifter; det är inte en torrkörning utan sidoeffekter.

## Snabbstart

Bygg och installera samlingen från denna utcheckning:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Filnamnet följer versionen i [galaxy.yml](../../../galaxy.yml). Använd filnamnet som bygget genererar om den versionen ändras.

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
        office: ["203.0.113.0/24"]  # Ersätt med kontorets nätverk (CIDR)
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

Granska den genererade policyn på en testvärd innan du tillämpar den på fjärrvärdar. Ha en oberoende återställningsväg vid ändring av brandväggspolicy.

## Utveckling

[CI-arbetsflödet](../../../.github/workflows/ci.yml) och [Molecule-scenarierna](../../../molecule) beskriver den konfigurerade valideringen. Deras förekomst bevisar inte lyckad driftsättning på varje distribution.

## Efterlevnad och licens

Den tidigare README-filen hänvisade till ett mappningsdokument för regelefterlevnad som inte finns i denna utcheckning. Ingen certifiering eller ramverksefterlevnad görs gällande här. Samlingen använder [MIT-licensen](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) dokumenterar paketets metadata.

## Stöd för operativsystem

Backend-implementeringen för nftables är testad med Ansible core 2.21 på Ubuntu 22.04, 24.04 och 26.04 LTS, Debian 12 och 13 samt Rocky Linux 9 och 10. Använd en underhållen kontrollnod med core 2.20 eller senare för Ubuntu 26.04:s Python 3.14.

Molecule kontrollerar konvergens, en andra körning utan ändringar, den inlästa regeluppsättningen, en tillåtelseregel för SSH-port 22, konfigurationsbehörigheter samt återställning efter ett injicerat tillämpningsfel. Enterprise Linux använder `/etc/sysconfig/nftables.conf`; Debian och Ubuntu använder `/etc/nftables.conf`. `nftables_conf_path` förblir konfigurerbar. Konfigurera den inbyggda nftables-tjänsten att läsa in en anpassad sökväg innan du använder den åsidosättningen: rollen startar om tjänsten, som annars läser in sin standardfil och kan ersätta den avsedda regeluppsättningen.

På Enterprise Linux stoppar en avvikande äldre `/etc/nftables.conf` rollen före aktivering. Säkerhetskopiera båda filerna och flytta sedan den validerade tidigare konfigurationen till den inbyggda tjänstens sökväg. Alternativt kan du uttryckligen behålla den äldre sökvägen och konfigurera tjänsten därefter. Lämna inte kvar en föråldrad kopia på `/etc/nftables.conf`: en avvikande fil där utlöser skyddet vid senare körningar. De tidigare reglerna förblir tillgängliga på den valda sökvägen som återställningskälla vid en uppgradering.

Rocky Linux testar paket- och tjänstesökvägen för Enterprise Linux. Dessa containertester certifierar inte RHEL-prenumerationer eller en produktionskärna. Ubuntu 20.04 utan utökat stöd, Debian 11 och EL8:s äldre Python ligger utanför den aktuella testmatrisen.
