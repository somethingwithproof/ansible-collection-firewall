<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · **Русский** · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · **Русский** · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Коллекция Ansible для описания политики межсетевого экрана хоста и применения конфигурации nftables. Группы адресов позволяют отделить политику от сгенерированного набора правил.

## Реализация и границы применимости

- [firewall](../../../roles/firewall/tasks/main.yml) отвечает за публичные значения по умолчанию и проверку бэкенда; [policy](../../../roles/policy/tasks/main.yml) сохраняет устаревшую точку входа, импортируя её.
- [nftables](../../../roles/nftables/tasks/main.yml) устанавливает nftables, подготавливает конфигурацию, проверяет её с помощью `nft -c -f` и применяет с помощью `nft -f`.
- **`nftables_backup: true`** — Блок применения пытается восстановить предыдущую конфигурацию после сбоя, если она существовала. Наличие резервной копии и доступа для восстановления остаётся эксплуатационным требованием.
- [Шаблон](../../../roles/nftables/templates/nftables.conf.j2) и [значения по умолчанию](../../../roles/firewall/defaults/main.yml) определяют поддерживаемые поля политики.

В состав коллекции входят роли `firewall`, `policy` и `nftables`; только `nftables` реализует бэкенд межсетевого экрана. Валидация политики принимает имена бэкендов iptables и firewalld, но соответствующие роли бэкендов в данную коллекцию не входят. Наличие правила SSH в шаблоне не гарантирует защиту от потери доступа. Режим только проверки (validate-only) по-прежнему устанавливает пакеты и выполняет задачи настройки и управления службами; это не пробный прогон без побочных эффектов.

## Быстрый старт

Соберите и установите коллекцию из этой локальной копии репозитория:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Имя файла зависит от версии, указанной в [galaxy.yml](../../../galaxy.yml). Если версия изменится, используйте имя файла, созданное при сборке.

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
        office: ["203.0.113.0/24"]  # Замените на сеть вашего офиса (CIDR)
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

Проверьте сгенерированную политику на тестовом хосте перед применением на удалённых хостах. При изменении политики межсетевого экрана обеспечьте независимый канал восстановления доступа.

## Разработка

[Рабочий процесс CI](../../../.github/workflows/ci.yml) и [сценарии Molecule](../../../molecule) описывают настроенную проверку. Их наличие не гарантирует успешное развёртывание на каждом дистрибутиве.

## Соответствие требованиям и лицензия

В предыдущей версии README упоминался документ сопоставления с нормативными требованиями, который отсутствует в этой версии репозитория. Заявления о сертификации или соответствии стандартам здесь не делаются. Коллекция распространяется под [лицензией MIT](../../../LICENSE); метаданные пакета указаны в [galaxy.yml](../../../galaxy.yml).

## Поддержка операционных систем

Бэкенд nftables протестирован с Ansible core 2.21 на Ubuntu 22.04, 24.04 и 26.04 LTS, Debian 12 и 13, а также Rocky Linux 9 и 10. Для Python 3.14 в Ubuntu 26.04 используйте поддерживаемый управляющий узел с core 2.20 или новее.

Molecule проверяет сходимость, повторный прогон без изменений, загруженный набор правил, разрешающее правило для SSH на порту 22, права доступа к конфигурации и откат после искусственно вызванного сбоя применения. Enterprise Linux использует `/etc/sysconfig/nftables.conf`; Debian и Ubuntu используют `/etc/nftables.conf`. Переменную `nftables_conf_path` можно переопределить. Прежде чем использовать это переопределение, настройте системную службу nftables на загрузку пользовательского пути: роль перезапускает службу, которая в противном случае загрузит свой стандартный файл и может заменить нужный набор правил.

В Enterprise Linux наличие отличающегося устаревшего файла `/etc/nftables.conf` останавливает выполнение роли до применения изменений. Сделайте резервные копии обоих файлов, затем переместите проверенную предыдущую конфигурацию по стандартному пути службы. В качестве альтернативы явно сохраните устаревший путь и настройте службу соответствующим образом. Не оставляйте устаревшую копию по пути `/etc/nftables.conf`: отличающийся файл в этом расположении приведёт к срабатыванию защитной проверки при последующих запусках. Предыдущие правила остаются доступными по выбранному пути в качестве источника отката при обновлении.

Rocky Linux используется для тестирования пакетов и путей служб Enterprise Linux. Эти контейнерные тесты не сертифицируют подписки RHEL или производственное ядро. Ubuntu 20.04 без расширенной поддержки, Debian 11 и более старая версия Python в EL8 находятся вне текущей матрицы тестирования.
