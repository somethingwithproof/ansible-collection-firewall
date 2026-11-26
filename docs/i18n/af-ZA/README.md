<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Afrikaans** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** **Afrikaans** · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

'n Ansible-versameling vir die beskrywing van gasheer-brandmuurbeleid en die toepassing van nftables-konfigurasie. Adresgroepe hou beleid apart van die gegenereerde reëlstel.

## Implementering en grense

- [firewall](../../../roles/firewall/tasks/main.yml) besit openbare verstekwaardes en agterkant-validering; [policy](../../../roles/policy/tasks/main.yml) behou die ouer toegangspunt deur dit in te voer.
- [nftables](../../../roles/nftables/tasks/main.yml) installeer nftables, berei konfigurasie voor, valideer dit met `nft -c -f`, en pas dit toe met `nft -f`.
- **`nftables_backup: true`** — Die toepassingsblok poog om 'n vorige konfigurasie ná 'n mislukking te herstel indien een bestaan het. Rugsteunbeskikbaarheid en hersteltoegang bly bedryfsvereistes.
- Die [sjabloon](../../../roles/nftables/templates/nftables.conf.j2) en [verstekwaardes](../../../roles/firewall/defaults/main.yml) definieer ondersteunde beleidsvelde.

Die versameling verpak `firewall`, `policy` en `nftables`; slegs `nftables` implementeer 'n brandmuur-agterkant. Agterkantname vir iptables en firewalld word deur beleidsvalidering aanvaar, maar geen ooreenstemmende agterkantrolle word hier verskaf nie. 'n SSH-reël in die sjabloon is nie 'n waarborg teen uitsluiting nie. Slegs-valideringsmodus installeer steeds pakkette en voer konfigurasie-/dienstake uit; dit is nie 'n newe-effekvrye proeflopie nie.

## Vinnige begin

Bou en installeer die versameling vanuit hierdie werkskopie:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Die lêernaam volg die weergawe in [galaxy.yml](../../../galaxy.yml). Gebruik die lêernaam wat deur die bouproses gegenereer word indien daardie weergawe verander.

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
        office: ["203.0.113.0/24"]  # Vervang met jou kantoornetwerk (CIDR)
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

Hersien die gegenereerde beleid op 'n toetsgasheer voordat dit op afgeleë gashere toegepas word. Behou 'n onafhanklike herstelpad wanneer brandmuurbeleid verander word.

## Ontwikkeling

Die [CI-werkvloei](../../../.github/workflows/ci.yml) en [Molecule-scenario's](../../../molecule) beskryf die gekonfigureerde validering. Hul teenwoordigheid bewys nie suksesvolle ontplooiing op elke verspreiding nie.

## Nakoming en lisensie

Die vorige README het verwys na 'n nakomingskarteringsdokument wat nie in hierdie werkskopie teenwoordig is nie. Geen sertifisering of raamwerknakoming word hier beweer nie. Die versameling gebruik die [MIT-lisensie](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) teken die pakketmetadata aan.

## Bedryfstelselondersteuning

Die nftables-agterkant word getoets met Ansible core 2.21 op Ubuntu 22.04,
24.04 en 26.04 LTS, Debian 12 en 13, en Rocky Linux 9 en 10. Gebruik 'n
onderhoude beheerder met core 2.20 of nuwer vir Ubuntu 26.04 se Python 3.14.

Molecule toets konvergensie, 'n tweede lopie sonder veranderinge, die gelaaide reëlstel,
'n SSH-poort 22-toelaatreël, konfigurasietoestemmings en terugrol ná 'n ingevoegde
toepassingsmislukking. Enterprise Linux gebruik `/etc/sysconfig/nftables.conf`; Debian en
Ubuntu gebruik `/etc/nftables.conf`. `nftables_conf_path` bly konfigureerbaar.
Konfigureer die inheemse nftables-diens om enige pasgemaakte pad te laai voordat
daardie oorheersing gebruik word: die rol herbegin die diens, wat andersins sy
versteklêer laai en die beoogde reëlstel kan vervang.

Op Enterprise Linux stop 'n afwykende ouer `/etc/nftables.conf` die rol
voor bevordering. Rugsteun albei lêers en skuif dan die gevalideerde vorige
konfigurasie na die inheemse dienspad. Alternatiewelik, behou die ouer pad
uitdruklik en konfigureer die diens dienooreenkomstig. Moenie 'n verouderde
kopie by `/etc/nftables.conf` agterlaat nie: 'n afwykende lêer daar aktiveer
die wagmeganisme tydens latere lopies. Die vorige reëls bly beskikbaar by
die gekose pad as die terugrolbron tydens 'n opgradering.

Rocky Linux toets die Enterprise Linux-pakket- en dienspad. Hierdie
houertoetse sertifiseer nie RHEL-intekeninge of 'n produksiekern nie.
Ubuntu 20.04 sonder uitgebreide ondersteuning, Debian 11 en EL8 se ouer
Python val buite die huidige toetsmatriks.
