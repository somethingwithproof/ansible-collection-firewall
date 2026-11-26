<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Kinyarwanda** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · **Kinyarwanda** · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Itsinda rya Ansible rigenewe gusobanura politiki ya firewall y'imashini no gushyira mu bikorwa igenamiterere rya nftables. Amatsinda y'aderesi atandukanya politiki n'urutonde rw'amategeko rwakozwe.

## Ishyirwa mu bikorwa n'imbibi

- [firewall](../../../roles/firewall/tasks/main.yml) igenga indangagaciro mbanziriza rusange n'igenzura rya backend; [policy](../../../roles/policy/tasks/main.yml) igumishaho uburyo bwo kwinjira bwa kera binyuze mu kuyitumiza.
- [nftables](../../../roles/nftables/tasks/main.yml) ishyiraho nftables, igategura igenamiterere, ikarigenzura ikoresheje `nft -c -f`, kandi ikarishyira mu bikorwa ikoresheje `nft -f`.
- Igice cyo gushyira mu bikorwa kigerageza gusubizaho igenamiterere ryari rihari mbere nyuma yo gutsindwa iyo ryari rihari. Kuboneka kw'ikopi y'ububiko n'uburyo bwo kugarura sisitemu bikomeza kuba ibisabwa mu mikorere.
- [Inyandikorugero](../../../roles/nftables/templates/nftables.conf.j2) n'[indangagaciro mbanziriza](../../../roles/policy/defaults/main.yml) bigaragaza ibice bya politiki byemewe.

Iri tsinda rikubiyemo `firewall`, `policy`, na `nftables`; `nftables` yonyine ni yo ishyira mu bikorwa backend ya firewall. Amazina ya backend ya iptables na firewalld yemerewe mu igenzura rya politiki, ariko nta nshingano za backend zijyanye na byo zitangwa hano. Itegeko rya SSH riri mu nyandikorugero si icyizere cyo kutafungirwa hanze. Uburyo bwo kugenzura gusa bukomeza gushyiraho paki kandi bugakora ibikorwa by'igenamiterere/serivisi; si igerageza ridahindura na kimwe.

## Gutangira byihuse

Wubake kandi ushyireho iri tsinda uhereye kuri iyi kopi y'ububiko:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Izina ry'idosiye rikurikiza verisiyo iri muri [galaxy.yml](../../../galaxy.yml). Koresha izina ry'idosiye ryavuye mu iyubakwa niba iyo verisiyo ihindutse.

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

Suzuma politiki yakozwe ku mashini y'igerageza mbere yo kuyishyira ku mashini za kure. Gumana inzira yigenga yo kugarura imikorere igihe uhindura politiki ya firewall.

## Iterambere

[Urukurikirane rw'ibikorwa bya CI](../../../.github/workflows/ci.yml) n'[ibyitegererezo bya Molecule](../../../molecule) bisobanura igenzura ryateguwe. Kubaho kwabyo ntibyemeza ko ishyirwa mu bikorwa rigenda neza kuri buri disitiribisiyo.

## Kubahiriza amategeko n'uruhushya

README ya mbere yavugaga inyandiko ihuza n'ibisabwa n'amategeko itaboneka muri iyi kopi y'ububiko. Nta cyemezo cyangwa kubahiriza urwego rw'amabwiriza byemezwa hano. Iri tsinda rikoresha [uruhushya rwa MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) ibika amakuru aranga paki.

## Sisitemu z'imikorere zishyigikiwe

Backend ya nftables yageragejwe na Ansible core 2.21 kuri Ubuntu 22.04,
24.04 na 26.04 LTS, Debian 12 na 13, na Rocky Linux 9 na 10. Koresha
umugenzuzi witaweho ufite core 2.20 cyangwa nshya kuri Python 3.14 ya Ubuntu 26.04.

Molecule igenzura ihuza ry'imikorere, ikorwa rya kabiri ridafite impinduka, urutonde rw'amategeko rwinjijwe,
itegeko ryemerera SSH ku muryango wa 22, uburenganzira bw'igenamiterere n'isubizainyuma nyuma y'ikosa ryatewe ku bushake mu gushyira mu bikorwa.
Enterprise Linux ikoresha `/etc/sysconfig/nftables.conf`; Debian na
Ubuntu zikoresha `/etc/nftables.conf`. `nftables_conf_path` ikomeza guhindurwa.
Tegura serivisi gakondo ya nftables kugira ngo ifungure inzira iyo ari yo yose yihariye mbere yo gukoresha
iryo hindura: inshingano itangiza serivisi bundi bushya, iyo bitabaye ibyo igafungura idosiye yayo
mbanziriza kandi ishobora gusimbura urutonde rw'amategeko rwifuzwa.

Kuri Enterprise Linux, idosiye ya kera itandukanye iri kuri `/etc/nftables.conf` ihagarika inshingano
mbere yo kwemeza impinduka. Bika ikopi y'amadosiye yombi, hanyuma wimure igenamiterere rya mbere
ryagenzuwe ku nzira gakondo ya serivisi. Cyangwa se, gumana ku mugaragaro
inzira ya kera maze utegure serivisi bijyanye na yo. Ntugasige ikopi ishaje
kuri `/etc/nftables.conf`: idosiye itandukanye iri aho ituma uburinzi bukora mu
bikorwa bizakurikira. Amategeko ya mbere akomeza kuboneka ku nzira yahiswemo nk'isoko
yo gusubizainyuma igihe cy'ivugurura.

Rocky Linux igerageza inzira za paki na serivisi za Enterprise Linux. Aya
magerageza yo muri container ntayemeza ifatabuguzi rya RHEL cyangwa kernel yo mu mikorere nyayo.
Ubuntu 20.04 itagira ubufasha bwongerewe, Debian 11 na Python ishaje ya EL8 biri
hanze y'urutonde rw'amagerageza rw'ubu.
