<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Kiswahili** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · **Kiswahili** · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Mkusanyiko wa Ansible wa kufafanua sera ya firewall ya mashine mwenyeji na kutumia usanidi wa nftables. Vikundi vya anwani hutenganisha sera na seti ya kanuni inayozalishwa.

## Utekelezaji na mipaka

- [firewall](../../../roles/firewall/tasks/main.yml) inasimamia mipangilio ya awali ya umma na uthibitishaji wa backend; [policy](../../../roles/policy/tasks/main.yml) huhifadhi njia ya awali ya kuingilia kwa kuiingiza.
- [nftables](../../../roles/nftables/tasks/main.yml) husakinisha nftables, huandaa usanidi kwa hatua, huuthibitisha kwa `nft -c -f`, na kuutekeleza kwa `nft -f`.
- Sehemu ya utekelezaji hujaribu kurejesha usanidi wa awali baada ya hitilafu iwapo ulikuwepo. Upatikanaji wa nakala rudufu na ufikiaji wa uokoaji hubaki kuwa mahitaji ya uendeshaji.
- [Kiolezo](../../../roles/nftables/templates/nftables.conf.j2) na [mipangilio ya awali](../../../roles/policy/defaults/main.yml) hufafanua sehemu za sera zinazotumika.

Mkusanyiko huu unajumuisha `firewall`, `policy`, na `nftables`; ni `nftables` pekee inayotekeleza backend ya firewall. Majina ya backend ya iptables na firewalld yanakubaliwa na uthibitishaji wa sera, lakini hakuna roles husika za backend zinazotolewa hapa. Kanuni ya SSH katika kiolezo si dhamana dhidi ya kufungiwa nje. Hali ya uthibitishaji pekee bado husakinisha vifurushi na kutekeleza kazi za usanidi/huduma; si jaribio lisilo na athari.

## Kuanza haraka

Jenga na usakinishe mkusanyiko kutoka nakala hii ya hazina:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Jina la faili hufuata toleo lililo katika [galaxy.yml](../../../galaxy.yml). Tumia jina la faili linalozalishwa na ujenzi iwapo toleo hilo litabadilika.

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

Kagua sera iliyozalishwa kwenye mashine ya majaribio kabla ya kuitumia kwenye mashine za mbali. Weka njia huru ya uokoaji unapobadilisha sera ya firewall.

## Uendelezaji

[Mtiririko wa kazi wa CI](../../../.github/workflows/ci.yml) na [matukio ya majaribio ya Molecule](../../../molecule) hufafanua uthibitishaji uliosanidiwa. Uwepo wake hauthibitishi utekelezaji wenye mafanikio kwenye kila toleo la mfumo.

## Uzingatiaji na leseni

README ya awali ilirejelea hati ya ulinganifu wa uzingatiaji ambayo haipo katika nakala hii ya hazina. Hakuna cheti wala uzingatiaji wa mfumo wowote unaodaiwa hapa. Mkusanyiko huu unatumia [leseni ya MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) hurekodi metadata ya kifurushi.

## Mifumo endeshi inayotumika

Backend ya nftables imejaribiwa na Ansible core 2.21 kwenye Ubuntu 22.04, 24.04 na 26.04 LTS, Debian 12 na 13, na Rocky Linux 9 na 10. Tumia kidhibiti kinachotunzwa chenye core 2.20 au toleo jipya zaidi kwa Python 3.14 ya Ubuntu 26.04.

Molecule hukagua utekelezaji kamili, uendeshaji wa pili bila mabadiliko, seti ya kanuni iliyopakiwa, kanuni ya kuruhusu SSH kwenye mlango 22, ruhusa za usanidi na urejeshaji wa hali ya awali baada ya hitilafu ya utekelezaji iliyoingizwa kwa makusudi. Enterprise Linux hutumia `/etc/sysconfig/nftables.conf`; Debian na Ubuntu hutumia `/etc/nftables.conf`. `nftables_conf_path` bado inaweza kusanidiwa. Sanidi huduma asili ya nftables kupakia njia yoyote maalum kabla ya kutumia mabadiliko hayo: role huanzisha upya huduma hiyo, ambayo vinginevyo hupakia faili yake ya msingi na inaweza kubadilisha seti ya kanuni iliyokusudiwa.

Kwenye Enterprise Linux, faili ya zamani ya `/etc/nftables.conf` inayotofautiana husimamisha role kabla ya uwasilishaji. Hifadhi nakala rudufu ya faili zote mbili, kisha hamisha usanidi wa awali uliothibitishwa hadi njia asili ya huduma. Vinginevyo, dumisha waziwazi njia ya zamani na usanidi huduma ipasavyo. Usiache nakala iliyopitwa na wakati katika `/etc/nftables.conf`: faili inayotofautiana hapo huanzisha ulinzi kwenye uendeshaji unaofuata. Kanuni za awali hubaki zinapatikana kwenye njia iliyochaguliwa kama chanzo cha kurejesha hali ya awali wakati wa usasishaji.

Rocky Linux hujaribu vifurushi na njia ya huduma ya Enterprise Linux. Majaribio haya ya kontena hayathibitishi usajili wa RHEL wala kernel ya uzalishaji. Ubuntu 20.04 isiyo na usaidizi uliopanuliwa, Debian 11 na Python ya zamani ya EL8 ziko nje ya jedwali la sasa la majaribio.
