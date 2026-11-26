<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Українська** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · **Українська** · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Колекція Ansible для опису політики брандмауера хоста та застосування конфігурації nftables. Групи адрес відокремлюють політику від згенерованого набору правил.

## Реалізація та межі

- [firewall](../../../roles/firewall/tasks/main.yml) відповідає за публічні значення за замовчуванням і перевірку бекенда; [policy](../../../roles/policy/tasks/main.yml) зберігає застарілу точку входу, імпортуючи її.
- [nftables](../../../roles/nftables/tasks/main.yml) встановлює nftables, готує конфігурацію, перевіряє її за допомогою `nft -c -f` і застосовує за допомогою `nft -f`.
- Блок застосування намагається відновити попередню конфігурацію після збою, якщо вона існувала. Наявність резервних копій і доступу для відновлення залишається експлуатаційною вимогою.
- [Шаблон](../../../roles/nftables/templates/nftables.conf.j2) і [значення за замовчуванням](../../../roles/policy/defaults/main.yml) визначають підтримувані поля політики.

Колекція містить ролі `firewall`, `policy` та `nftables`; лише `nftables` реалізує бекенд брандмауера. Перевірка політики приймає назви бекендів iptables і firewalld, але відповідні ролі бекендів тут не постачаються. Правило SSH у шаблоні не гарантує захисту від блокування доступу. Режим лише перевірки все одно встановлює пакети та виконує завдання налаштування й керування службами; це не пробний запуск без побічних ефектів.

## Швидкий старт

Зберіть і встановіть колекцію з цієї локальної копії репозиторію:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Ім'я файлу відповідає версії в [galaxy.yml](../../../galaxy.yml). Якщо ця версія зміниться, використовуйте ім'я файлу, створене під час збірки.

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

Перевірте згенеровану політику на тестовому хості перед застосуванням її на віддалених хостах. Зберігайте незалежний канал відновлення доступу під час зміни політики брандмауера.

## Розробка

[Робочий процес CI](../../../.github/workflows/ci.yml) та [сценарії Molecule](../../../molecule) описують налаштовану перевірку. Їхня наявність не доводить успішного розгортання в кожному дистрибутиві.

## Відповідність вимогам і ліцензія

Попередня версія README посилалася на документ зіставлення вимог відповідності, якого немає в цій локальній копії репозиторію. Тут не заявляється жодна сертифікація чи відповідність стандартам. Колекція поширюється за [ліцензією MIT](../../../LICENSE); метадані пакета зафіксовано в [galaxy.yml](../../../galaxy.yml).

## Підтримка операційних систем

Бекенд nftables тестується з Ansible core 2.21 на Ubuntu 22.04, 24.04 і 26.04 LTS, Debian 12 і 13, а також Rocky Linux 9 і 10. Для Python 3.14 в Ubuntu 26.04 використовуйте підтримуваний контролер із core 2.20 або новішої версії.

Molecule перевіряє збіжність, повторний запуск без змін, завантажений набір правил, правило дозволу для порту SSH 22, права доступу до конфігурації та відкат після змодельованого збою застосування. Enterprise Linux використовує `/etc/sysconfig/nftables.conf`; Debian і Ubuntu використовують `/etc/nftables.conf`. Змінна `nftables_conf_path` залишається налаштовуваною. Перед використанням власного шляху налаштуйте системну службу nftables на його завантаження: роль перезапускає службу, яка інакше завантажить стандартний файл і може замінити потрібний набір правил.

В Enterprise Linux наявність відмінного застарілого файлу `/etc/nftables.conf` зупиняє виконання ролі перед активацією конфігурації. Створіть резервні копії обох файлів, а потім перемістіть перевірену попередню конфігурацію за системним шляхом служби. Як альтернативу явно збережіть застарілий шлях і відповідно налаштуйте службу. Не залишайте застарілу копію в `/etc/nftables.conf`: відмінний файл за цим шляхом активує запобіжник під час наступних запусків. Попередні правила залишаються доступними за вибраним шляхом як джерело для відкату під час оновлення.

Rocky Linux перевіряє шлях пакетів і служб для Enterprise Linux. Ці контейнерні тести не сертифікують підписки RHEL або промислове ядро. Ubuntu 20.04 без розширеної підтримки, Debian 11 та старіша версія Python в EL8 не входять до поточної матриці тестування.
