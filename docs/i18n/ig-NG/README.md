<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Igbo** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · **Igbo** · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Mkpokọta Ansible maka ịkọwa atụmatụ firewall nke host na itinye nhazi nftables n'ọrụ. Otu adreesị na-ekewapụ atụmatụ pụọ na usoro iwu a rụpụtara.

## Mmejupụta na oke ọrụ

- [firewall](../../../roles/firewall/tasks/main.yml) nwe ntọala ndabara ọha na nnyocha nke backend; [policy](../../../roles/policy/tasks/main.yml) na-echekwa ebe mbanye ochie site n'ibubata ya.
- [nftables](../../../roles/nftables/tasks/main.yml) na-awụnye nftables, na-akwado nhazi, na-enyocha ya site na `nft -c -f`, ma na-etinye ya n'ọrụ site na `nft -f`.
- **`nftables_backup: true`** — Ngalaba ntinye n'ọrụ na-anwa iweghachi nhazi gara aga mgbe ọdịda mere ma ọ bụrụ na otu dịbu. Ịdị njikere nke nchekwa ndabere na ụzọ mgbake ka bụ ihe a chọrọ n'ọrụ.
- [Ndebiri](../../../roles/nftables/templates/nftables.conf.j2) na [ntọala ndabara](../../../roles/firewall/defaults/main.yml) na-akọwa ngalaba atụmatụ ndị a kwadoro.

Mkpokọta a na-akwakọ `firewall`, `policy`, na `nftables`; naanị `nftables` na-emejupụta backend nke firewall. Nnyocha atụmatụ na-anabata aha backend maka iptables na firewalld, mana enweghị ọrụ backend dabara adaba e tinyere ebe a. Iwu SSH dị na ndebiri ahụ abụghị nkwa megide mkpọchi. Ọnọdụ nnyocha naanị ka na-awụnye ngwugwu ma na-arụ ọrụ nhazi/ọrụ usoro; ọ bụghị nnwale na-enweghị mmetụta ọ bụla.

## Mmalite ngwa ngwa

Wuo ma wụnye mkpokọta a site na nkopi koodu a:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Aha faịlụ ahụ na-eso ụdị dị na [galaxy.yml](../../../galaxy.yml). Jiri aha faịlụ nke mwulite ahụ rụpụtara ma ọ bụrụ na ụdị ahụ agbanwee.

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
        office: ["203.0.113.0/24"]  # Jiri netwọkụ ụlọ ọrụ gị dochie nke a (CIDR)
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

Lelee atụmatụ a rụpụtara na host nnwale tupu itinye ya na host ndị dị anya. Dobe ụzọ mgbake nọọrọ onwe ya mgbe ị na-agbanwe atụmatụ firewall.

## Mmepe

[Usoro ọrụ CI](../../../.github/workflows/ci.yml) na [ọnọdụ nnwale Molecule](../../../molecule) na-akọwa nnyocha e haziri. Ịdị ha adị egosighị na ntinye n'ọrụ ga-aga nke ọma na nkesa ọ bụla.

## Nrubeisi na ikikere

README gara aga zoro aka n'akwụkwọ nkọwa nrubeisi nke na-adịghị na nkopi koodu a. Enweghị asambodo ma ọ bụ nrubeisi n'ụkpụrụ ọ bụla a na-ekwu ebe a. Mkpokọta a na-eji [ikikere MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) na-edekọ metadata nke ngwugwu ahụ.

## Nkwado sistemụ arụmọrụ

A nwalere backend nke nftables na Ansible core 2.21 na Ubuntu 22.04,
24.04 na 26.04 LTS, Debian 12 na 13, yana Rocky Linux 9 na 10. Jiri
controller a na-elekọta nke nwere core 2.20 ma ọ bụ nke ọhụrụ karịa maka Python 3.14 nke Ubuntu 26.04.

Molecule na-enyocha nchịkọta, ọsọ nke abụọ na-enweghị mgbanwe, usoro iwu e buru n'ime,
iwu nkwenye SSH nke ọdụ 22, ikike nhazi na nlọghachi azụ mgbe ọdịda ntinye e tinyere
n'ebumnuche mere. Enterprise Linux na-eji `/etc/sysconfig/nftables.conf`; Debian na
Ubuntu na-eji `/etc/nftables.conf`. `nftables_conf_path` ka bụ ihe a pụrụ ịhazi.
Hazie ọrụ nftables nke sistemụ ahụ ka o buo ụzọ ọ bụla a haziri iche tupu i jiri
ngbanwe ahụ: ọrụ ahụ na-amalitegharị ọrụ usoro ahụ, nke ga-ebu faịlụ ndabara ya ma ọ bụghị ya,
ma nwee ike dochie usoro iwu e zubere.

Na Enterprise Linux, faịlụ ochie `/etc/nftables.conf` dị iche na-akwụsị ọrụ ahụ
tupu nkwalite. Chekwaa faịlụ abụọ ahụ, wee bufee nhazi gara aga a nyochara
n'ụzọ ọrụ nke sistemụ ahụ. N'aka nke ọzọ, dobe
ụzọ ochie ahụ n'ụzọ doro anya ma hazie ọrụ usoro ahụ otu o kwesịrị. Ahapụla nkopi ochie
na `/etc/nftables.conf`: faịlụ dị iche n'ebe ahụ na-akpalite nchedo
n'ọsọ ndị ga-abịa n'ihu. Iwu ndị gara aga ka dị n'ụzọ a họọrọ dị ka
isi iyi nlọghachi azụ n'oge nkwalite.

Rocky Linux na-anwale ụzọ ngwugwu na ọrụ nke Enterprise Linux. Nnwale
container ndị a anaghị enye asambodo maka ndenye aha RHEL ma ọ bụ kernel mmepụta.
Ubuntu 20.04 na-enweghị nkwado gbatịrị agbatị, Debian 11 na Python ochie nke EL8
dị na mpụga usoro nnwale dị ugbu a.
