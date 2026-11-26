<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Yorùbá** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · **Yorùbá** · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Àkójọpọ̀ Ansible fún ṣíṣàpèjúwe ìlànà ògiri-iná ẹ̀rọ olùgbàlejò àti mímú ìṣètò nftables ṣiṣẹ́. Àwọn ẹgbẹ́ àdírẹ́sì máa ń pa ìlànà mọ́ lọ́tọ̀ kúrò lára àkójọ òfin tí a ṣẹ̀dá.

## Ìmúṣẹ àti àwọn ààlà

- [firewall](../../../roles/firewall/tasks/main.yml) ni ó ni àwọn àṣàyàn àkọ́kọ́ fún gbogbo ènìyàn àti ìjẹ́rìísí ẹ̀yìn-ètò; [policy](../../../roles/policy/tasks/main.yml) máa ń pa ojú-ọ̀nà ìbẹ̀rẹ̀ àtijọ́ mọ́ nípa gbígbé e wọlé.
- [nftables](../../../roles/nftables/tasks/main.yml) máa ń fi nftables sórí ẹ̀rọ, ó ń gbé ìṣètò kalẹ̀ fún ìpalẹ̀mọ́, ó ń jẹ́rìísí rẹ̀ pẹ̀lú `nft -c -f`, ó sì ń mú un ṣiṣẹ́ pẹ̀lú `nft -f`.
- **`nftables_backup: true`** — Abala ìmúlò máa ń gbìyànjú láti dá ìṣètò tẹ́lẹ̀ padà lẹ́yìn ìkùnà nígbà tí ọ̀kan bá wà tẹ́lẹ̀. Wíwà àdàkọ ààbò àti àǹfààní ìgbàpadà ṣì jẹ́ àwọn ohun tí a nílò nínú ìṣiṣẹ́.
- [Àwòkọ́ṣe](../../../roles/nftables/templates/nftables.conf.j2) àti [àwọn àṣàyàn àkọ́kọ́](../../../roles/firewall/defaults/main.yml) ni ó ń ṣàlàyé àwọn pápá ìlànà tí a ṣètìlẹ́yìn fún.

Àkójọpọ̀ yìí kó `firewall`, `policy`, àti `nftables` jọ; `nftables` nìkan ni ó ń ṣe ìmúṣẹ ẹ̀yìn-ètò ògiri-iná. Ìjẹ́rìísí ìlànà máa ń gba àwọn orúkọ ẹ̀yìn-ètò fún iptables àti firewalld, ṣùgbọ́n kò sí àwọn ipa ẹ̀yìn-ètò tí ó bá wọn mu tí a fi ránṣẹ́ níbí. Òfin SSH nínú àwòkọ́ṣe kì í ṣe ìdánilójú pé ìtìpa-kúrò kò ní ṣẹlẹ̀. Ipò ìjẹ́rìísí-nìkan ṣì ń fi àwọn àpò sórí ẹ̀rọ, ó sì ń ṣe àwọn iṣẹ́ ìṣètò/iṣẹ́-ìpèsè; kì í ṣe ìdánwò láìsí ipa-ẹ̀gbẹ́.

## Ìbẹ̀rẹ̀ kíákíá

Kọ àkójọpọ̀ náà kí o sì fi í sórí ẹ̀rọ láti inú àdàkọ ìṣiṣẹ́ yìí:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Orúkọ fáìlì náà tẹ̀lé ẹ̀yà tí ó wà nínú [galaxy.yml](../../../galaxy.yml). Lo orúkọ fáìlì tí ìkọ́lé náà mú jáde bí ẹ̀yà náà bá yípadà.

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
        office: ["203.0.113.0/24"]  # Fi nọ́ńbà nẹ́tíwọ́ọ̀kì ọ́fíìsì rẹ sí ibí yìí (CIDR)
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

Ṣàyẹ̀wò ìlànà tí a ṣẹ̀dá lórí ẹ̀rọ olùgbàlejò ìdánwò kí o tó mú un ṣiṣẹ́ lórí àwọn ẹ̀rọ olùgbàlejò jíjìn. Pa ọ̀nà ìgbàpadà olómìnira mọ́ nígbà tí o bá ń yí ìlànà ògiri-iná padà.

## Ìdàgbàsókè

[Ìṣàn-iṣẹ́ CI](../../../.github/workflows/ci.yml) àti [àwọn ìṣẹ̀lẹ̀ Molecule](../../../molecule) ṣàpèjúwe ìjẹ́rìísí tí a ṣètò. Wíwà wọn kò fìdí ìgbékalẹ̀ àṣeyọrí múlẹ̀ lórí gbogbo ìpínkiri ètò ìṣiṣẹ́.

## Ìbámu àti ìwé-àṣẹ

README tẹ́lẹ̀ tọ́ka sí ìwé ìfiwéra-ìbámu kan tí kò sí nínú àdàkọ ìṣiṣẹ́ yìí. A kò sọ pé a ní ìwé-ẹ̀rí tàbí ìbámu ìlànà èyíkéyìí níbí. Àkójọpọ̀ náà ń lo [ìwé-àṣẹ MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) ni ó ń ṣàkọsílẹ̀ àwọn dátà-àpèjúwe àpò náà.

## Àtìlẹ́yìn ètò ìṣiṣẹ́

A dán ẹ̀yìn-ètò nftables wò pẹ̀lú Ansible core 2.21 lórí Ubuntu 22.04, 24.04 àti 26.04 LTS, Debian 12 àti 13, àti Rocky Linux 9 àti 10. Lo olùṣàkóso tí a ń tọ́jú pẹ̀lú core 2.20 tàbí ẹ̀yà tuntun jù bẹ́ẹ̀ lọ fún Python 3.14 ti Ubuntu 26.04.

Molecule máa ń ṣàyẹ̀wò ìpéjọpọ̀, ìṣiṣẹ́ kejì láìsí àyípadà, àkójọ òfin tí a gbé wọlé, òfin ìfàyègba ibùdó SSH 22, àwọn àṣẹ ìṣètò, àti ìpadàsẹ́yìn lẹ́yìn ìkùnà ìmúlò tí a fi sínú rẹ̀ mọ̀ọ́mọ̀. Enterprise Linux ń lo `/etc/sysconfig/nftables.conf`; Debian àti Ubuntu ń lo `/etc/nftables.conf`. `nftables_conf_path` ṣì ṣeé ṣètò. Ṣètò iṣẹ́-ìpèsè nftables abínibí láti gbé ọ̀nà àdáni èyíkéyìí wọlé kí o tó lo àyípadà yẹn: ipa náà máa ń tún iṣẹ́-ìpèsè náà bẹ̀rẹ̀, èyí tí yóò gbé fáìlì àkọ́kọ́ rẹ̀ wọlé bí bẹ́ẹ̀ kọ́, tí ó sì lè rọ́pò àkójọ òfin tí a pinnu.

Lórí Enterprise Linux, `/etc/nftables.conf` àtijọ́ tí ó yàtọ̀ máa ń dá ipa náà dúró kí ìgbéga tó ṣẹlẹ̀. Ṣe àdàkọ ààbò fáìlì méjèèjì, lẹ́yìn náà gbé ìṣètò tẹ́lẹ̀ tí a ti jẹ́rìísí lọ sí ọ̀nà iṣẹ́-ìpèsè abínibí. Ní ọ̀nà mìíràn, ṣe ìpinnu kedere láti pa ọ̀nà àtijọ́ mọ́ kí o sì ṣètò iṣẹ́-ìpèsè náà gẹ́gẹ́ bí ó ṣe yẹ. Má ṣe fi àdàkọ àtijọ́ tí kò wúlò mọ́ sílẹ̀ ní `/etc/nftables.conf`: fáìlì tí ó yàtọ̀ níbẹ̀ máa ń fa ìṣọ́ra dúró lórí àwọn ìṣiṣẹ́ tó tẹ̀lé e. Àwọn òfin tẹ́lẹ̀ ṣì wà ní ọ̀nà tí a yàn gẹ́gẹ́ bí orísun ìpadàsẹ́yìn nígbà ìgbéga.

Rocky Linux ń dán ọ̀nà àpò àti iṣẹ́-ìpèsè Enterprise Linux wò. Àwọn ìdánwò àpótí-ẹ̀rọ wọ̀nyí kò fọwọ́sí ìforúkọsílẹ̀ RHEL tàbí kánẹ́ẹ̀lì ìṣẹ̀dá gidi. Ubuntu 20.04 láìsí àtìlẹ́yìn gbígbòòrò, Debian 11, àti Python àtijọ́ ti EL8 wà níta àtẹ ìdánwò lọ́wọ́lọ́wọ́.
