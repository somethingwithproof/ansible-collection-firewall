<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Deutsch** · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · **Deutsch** · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Eine Ansible-Collection zum Beschreiben von Host-Firewall-Richtlinien und zum Anwenden von nftables-Konfigurationen. Adressgruppen trennen die Richtlinie vom generierten Regelwerk.

## Implementierung und Grenzen

- [firewall](../../../roles/firewall/tasks/main.yml) verwaltet die öffentlichen Standardwerte und die Backend-Validierung; [policy](../../../roles/policy/tasks/main.yml) erhält den Legacy-Einstiegspunkt durch dessen Import.
- [nftables](../../../roles/nftables/tasks/main.yml) installiert nftables, stellt die Konfiguration bereit, validiert sie mit `nft -c -f` und wendet sie mit `nft -f` an.
- **`nftables_backup: true`** — Der Apply-Block versucht nach einem Fehler, eine zuvor vorhandene Konfiguration wiederherzustellen. Die Verfügbarkeit von Backups und ein Wiederherstellungszugang bleiben betriebliche Voraussetzungen.
- Die [Vorlage](../../../roles/nftables/templates/nftables.conf.j2) und die [Standardwerte](../../../roles/firewall/defaults/main.yml) definieren die unterstützten Richtlinienfelder.

Die Collection paketiert `firewall`, `policy` und `nftables`; nur `nftables` implementiert ein Firewall-Backend. Backend-Namen für iptables und firewalld werden von der Richtlinienvalidierung akzeptiert, entsprechende Backend-Rollen sind hier jedoch nicht enthalten. Eine SSH-Regel in der Vorlage ist keine Garantie gegen eine Aussperrung. Der reine Validierungsmodus installiert dennoch Pakete und führt Konfigurations- und Dienstaufgaben aus; er ist kein nebenwirkungsfreier Probelauf.

## Schnellstart

Collection aus diesem Checkout erstellen und installieren:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Der Dateiname richtet sich nach der Version in [galaxy.yml](../../../galaxy.yml). Verwenden Sie den beim Build erzeugten Dateinamen, falls sich diese Version ändert.

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
        office: ["203.0.113.0/24"]  # Durch das eigene Büronetz ersetzen (CIDR)
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

Prüfen Sie die generierte Richtlinie auf einem Testhost, bevor Sie sie auf entfernte Hosts anwenden. Halten Sie bei Änderungen an Firewall-Richtlinien stets einen unabhängigen Wiederherstellungszugang bereit.

## Entwicklung

Der [CI-Workflow](../../../.github/workflows/ci.yml) und die [Molecule-Szenarien](../../../molecule) beschreiben die konfigurierte Validierung. Ihr Vorhandensein garantiert keine erfolgreiche Bereitstellung auf jeder Distribution.

## Compliance und Lizenz

Das frühere README verwies auf ein Dokument zur Compliance-Zuordnung, das in diesem Checkout nicht vorhanden ist. Es wird hier weder eine Zertifizierung noch die Einhaltung bestimmter Frameworks beansprucht. Die Collection steht unter der [MIT-Lizenz](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) enthält die Paketmetadaten.

## Betriebssystemunterstützung

Das nftables-Backend wird mit Ansible Core 2.21 auf Ubuntu 22.04, 24.04 und 26.04 LTS, Debian 12 und 13 sowie Rocky Linux 9 und 10 getestet. Verwenden Sie für Python 3.14 unter Ubuntu 26.04 einen gewarteten Controller mit Core 2.20 oder neuer.

Molecule prüft die Konvergenz, einen zweiten Durchlauf ohne Änderungen, das geladene Regelwerk, eine SSH-Freigaberegel für Port 22, Konfigurationsberechtigungen sowie das Rollback nach einem gezielt ausgelösten Fehler beim Anwenden. Enterprise Linux verwendet `/etc/sysconfig/nftables.conf`; Debian und Ubuntu verwenden `/etc/nftables.conf`. `nftables_conf_path` bleibt konfigurierbar. Konfigurieren Sie den nativen nftables-Dienst so, dass er einen benutzerdefinierten Pfad lädt, bevor Sie diese Überschreibung nutzen: Die Rolle startet den Dienst neu, der andernfalls seine Standarddatei lädt und das beabsichtigte Regelwerk überschreiben kann.

Unter Enterprise Linux stoppt eine abweichende Legacy-Datei `/etc/nftables.conf` die Rolle vor der Übernahme. Sichern Sie beide Dateien und verschieben Sie anschließend die validierte vorherige Konfiguration an den nativen Dienstpfad. Alternativ können Sie den Legacy-Pfad explizit beibehalten und den Dienst entsprechend konfigurieren. Belassen Sie keine veraltete Kopie unter `/etc/nftables.conf`: Eine dort vorhandene abweichende Datei löst die Schutzprüfung bei späteren Durchläufen aus. Die bisherigen Regeln bleiben am gewählten Pfad während eines Upgrades als Rollback-Quelle verfügbar.

Rocky Linux testet den Paket- und Dienstpfad von Enterprise Linux. Diese Container-Tests zertifizieren weder RHEL-Subskriptionen noch einen Produktiv-Kernel. Ubuntu 20.04 ohne erweiterten Support, Debian 11 sowie das ältere Python von EL8 liegen außerhalb der aktuellen Testmatrix.
