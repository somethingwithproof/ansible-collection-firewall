<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Nederlands** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · **Nederlands** · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Een Ansible-collectie voor het beschrijven van host-firewallbeleid en het toepassen van nftables-configuratie. Adresgroepen houden het beleid gescheiden van de gegenereerde regelset.

## Implementatie en grenzen

- [firewall](../../../roles/firewall/tasks/main.yml) beheert de publieke standaardwaarden en backendvalidatie; [policy](../../../roles/policy/tasks/main.yml) behoudt het oude startpunt door dit te importeren.
- [nftables](../../../roles/nftables/tasks/main.yml) installeert nftables, zet de configuratie klaar, valideert deze met `nft -c -f` en past deze toe met `nft -f`.
- **`nftables_backup: true`** — Het toepassingsblok probeert na een fout een eerdere configuratie te herstellen indien deze bestond. De beschikbaarheid van back-ups en hersteltoegang blijven operationele vereisten.
- Het [sjabloon](../../../roles/nftables/templates/nftables.conf.j2) en de [standaardwaarden](../../../roles/firewall/defaults/main.yml) definiëren de ondersteunde beleidsvelden.

De collectie bevat de rollen `firewall`, `policy` en `nftables`; alleen `nftables` implementeert een firewall-backend. Backendnamen voor iptables en firewalld worden geaccepteerd door de beleidsvalidatie, maar bijbehorende backendrollen worden hier niet meegeleverd. Een SSH-regel in het sjabloon is geen garantie tegen buitensluiting. De modus alleen-valideren installeert nog steeds pakketten en voert configuratie- en servicetaken uit; het is geen bijwerkingenvrije dry run.

## Snelstart

Bouw en installeer de collectie vanuit deze checkout:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

De bestandsnaam volgt de versie in [galaxy.yml](../../../galaxy.yml). Gebruik de bestandsnaam die door de build wordt geproduceerd als die versie verandert.

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
        office: ["203.0.113.0/24"]  # Vervang dit door het eigen kantoornetwerk (CIDR)
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

Controleer het gegenereerde beleid op een testhost voordat je het toepast op externe hosts. Zorg voor een onafhankelijk herstelpad bij het wijzigen van firewallbeleid.

## Ontwikkeling

De [CI-workflow](../../../.github/workflows/ci.yml) en [Molecule-scenario's](../../../molecule) beschrijven de geconfigureerde validatie. Hun aanwezigheid bewijst geen succesvolle uitrol op elke distributie.

## Compliance en licentie

De eerdere README verwees naar een compliance-mappingdocument dat niet aanwezig is in deze checkout. Er wordt hier geen certificering of naleving van frameworks geclaimd. De collectie gebruikt de [MIT-licentie](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) legt de pakketmetadata vast.

## Ondersteuning van besturingssystemen

De nftables-backend is getest met Ansible core 2.21 op Ubuntu 22.04, 24.04 en 26.04 LTS, Debian 12 en 13, en Rocky Linux 9 en 10. Gebruik een onderhouden controller met core 2.20 of nieuwer voor Python 3.14 op Ubuntu 26.04.

Molecule controleert convergentie, een tweede uitvoering zonder wijzigingen, de geladen regelset, een toegangsregel voor SSH-poort 22, configuratierechten en rollback na een geïnjecteerde toepassingsfout. Enterprise Linux gebruikt `/etc/sysconfig/nftables.conf`; Debian en Ubuntu gebruiken `/etc/nftables.conf`. `nftables_conf_path` blijft configureerbaar. Configureer de native nftables-service om een aangepast pad te laden voordat je die overschrijving gebruikt: de rol herstart de service, die anders het standaardbestand laadt en de beoogde regelset kan vervangen.

Op Enterprise Linux stopt een afwijkend verouderd `/etc/nftables.conf` de rol vóór promotie. Maak een back-up van beide bestanden en verplaats vervolgens de gevalideerde vorige configuratie naar het native servicepad. Je kunt er ook voor kiezen het verouderde pad expliciet te behouden en de service dienovereenkomstig te configureren. Laat geen verouderde kopie achter op `/etc/nftables.conf`: een afwijkend bestand op die locatie activeert de beveiligingscontrole bij latere uitvoeringen. De vorige regels blijven beschikbaar op het geselecteerde pad als rollback-bron tijdens een upgrade.

Rocky Linux test het pakket- en servicepad van Enterprise Linux. Deze containertests certificeren geen RHEL-abonnementen of een productiekernel. Ubuntu 20.04 zonder uitgebreide ondersteuning, Debian 11 en de oudere Python-versie van EL8 vallen buiten de huidige testmatrix.
