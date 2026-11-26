<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Polski** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · **Polski** · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Kolekcja Ansible do opisywania polityki zapory sieciowej hosta i wdrażania konfiguracji nftables. Grupy adresów oddzielają politykę od generowanego zestawu reguł.

## Implementacja i granice zakresu

- Rola [firewall](../../../roles/firewall/tasks/main.yml) odpowiada za publiczne wartości domyślne i walidację backendu; rola [policy](../../../roles/policy/tasks/main.yml) zachowuje starszy punkt wejścia, importując ją.
- Rola [nftables](../../../roles/nftables/tasks/main.yml) instaluje nftables, przygotowuje konfigurację przejściową, waliduje ją za pomocą `nft -c -f` i stosuje za pomocą `nft -f`.
- Blok stosowania konfiguracji podejmuje próbę przywrócenia poprzedniej konfiguracji w razie niepowodzenia, jeśli taka istniała. Dostępność kopii zapasowej oraz awaryjnego dostępu pozostają wymaganiami operacyjnymi.
- [Szablon](../../../roles/nftables/templates/nftables.conf.j2) i [wartości domyślne](../../../roles/policy/defaults/main.yml) definiują obsługiwane pola polityki.

Kolekcja zawiera role `firewall`, `policy` oraz `nftables`; jedynie `nftables` implementuje backend zapory sieciowej. Nazwy backendów dla iptables i firewalld są akceptowane podczas walidacji polityki, jednak nie są tu dostarczane odpowiadające im role backendów. Reguła SSH w szablonie nie gwarantuje ochrony przed utratą dostępu. Tryb samej walidacji nadal instaluje pakiety i wykonuje zadania konfiguracji oraz usług; nie jest to uruchomienie próbne pozbawione efektów ubocznych.

## Szybki start

Zbuduj i zainstaluj kolekcję z tej kopii roboczej:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Nazwa pliku odpowiada wersji w [galaxy.yml](../../../galaxy.yml). Jeśli ta wersja ulegnie zmianie, użyj nazwy pliku wygenerowanej podczas budowania.

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

Sprawdź wygenerowaną politykę na hoście testowym przed jej wdrożeniem na zdalnych hostach. Podczas zmiany polityki zapory sieciowej zachowaj niezależną ścieżkę odzyskiwania dostępu.

## Rozwój

[Przepływ pracy CI](../../../.github/workflows/ci.yml) oraz [scenariusze Molecule](../../../molecule) opisują skonfigurowaną walidację. Ich obecność nie potwierdza pomyślnego wdrożenia w każdej dystrybucji.

## Zgodność i licencja

Wcześniejsza wersja pliku README odnosiła się do dokumentu mapowania zgodności, którego nie ma w tej kopii roboczej. Nie deklaruje się tu żadnej certyfikacji ani zgodności z ramami regulacyjnymi. Kolekcja jest udostępniana na [licencji MIT](../../../LICENSE); plik [galaxy.yml](../../../galaxy.yml) zawiera metadane pakietu.

## Obsługa systemów operacyjnych

Backend nftables jest testowany z Ansible core 2.21 w systemach Ubuntu 22.04,
24.04 i 26.04 LTS, Debian 12 i 13 oraz Rocky Linux 9 i 10. Dla Pythona 3.14
w Ubuntu 26.04 używaj wspieranego kontrolera z Ansible core 2.20 lub nowszym.

Molecule sprawdza konwergencję, drugie uruchomienie bez zmian, załadowany zestaw reguł,
regułę zezwalającą na ruch SSH na porcie 22, uprawnienia plików konfiguracji oraz wycofanie zmian po wstrzykniętym błędzie stosowania konfiguracji.
Enterprise Linux używa `/etc/sysconfig/nftables.conf`; Debian i Ubuntu
używają `/etc/nftables.conf`. Zmienna `nftables_conf_path` pozostaje konfigurowalna.
Przed użyciem niestandardowej ścieżki skonfiguruj natywną usługę nftables tak, aby ją ładowała:
rola restartuje tę usługę, która w przeciwnym razie załaduje domyślny plik i może zastąpić zamierzony zestaw reguł.

W systemach Enterprise Linux różniący się starszy plik `/etc/nftables.conf` zatrzymuje rolę
przed wdrożeniem nowej konfiguracji. Utwórz kopie zapasowe obu plików, a następnie przenieś zweryfikowaną
poprzednią konfigurację do natywnej ścieżki usługi. Alternatywnie jawnie zachowaj starszą ścieżkę
i odpowiednio skonfiguruj usługę. Nie zostawiaj nieaktualnej kopii w `/etc/nftables.conf`:
różniący się plik w tym miejscu uruchomi zabezpieczenie przy kolejnych uruchomieniach. Poprzednie reguły
pozostają dostępne w wybranej ścieżce jako źródło wycofania zmian podczas aktualizacji.

Rocky Linux weryfikuje ścieżkę pakietów i usług dla Enterprise Linux. Te testy kontenerowe
nie certyfikują subskrypcji RHEL ani jądra produkcyjnego. Ubuntu 20.04 bez rozszerzonego wsparcia,
Debian 11 oraz starszy Python w EL8 znajdują się poza bieżącą macierzą testową.
