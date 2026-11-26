<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Filipino** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · **Filipino**

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Isang koleksiyon ng Ansible para sa paglalarawan ng patakaran ng firewall ng host at paglalapat ng konfigurasyon ng nftables. Pinananatili ng mga pangkat ng address na hiwalay ang patakaran mula sa nalikhang ruleset.

## Implementasyon at mga hangganan

- Ang [firewall](../../../roles/firewall/tasks/main.yml) ang nagmamay-ari ng mga pampublikong default at pagpapatunay ng backend; pinananatili ng [policy](../../../roles/policy/tasks/main.yml) ang legacy entry point sa pamamagitan ng pag-import nito.
- Ang [nftables](../../../roles/nftables/tasks/main.yml) ay nag-i-install ng nftables, naghahanda ng konfigurasyon, nagpapatunay nito gamit ang `nft -c -f`, at naglalapat nito gamit ang `nft -f`.
- Sinusubukan ng apply block na ibalik ang nakaraang konfigurasyon pagkatapos ng isang pagkabigo kapag mayroon nito. Ang pagkakaroon ng backup at access sa pagbawi ay nananatiling mga kinakailangan sa operasyon.
- Tinutukoy ng [template](../../../roles/nftables/templates/nftables.conf.j2) at [mga default](../../../roles/policy/defaults/main.yml) ang mga sinusuportahang field ng patakaran.

Kasama sa koleksiyon ang `firewall`, `policy`, at `nftables`; tanging ang `nftables` lamang ang nagpapatupad ng backend ng firewall. Ang mga pangalan ng backend para sa iptables at firewalld ay tinatanggap ng pagpapatunay ng patakaran, ngunit walang kaukulang mga role ng backend ang kasama rito. Ang isang panuntunan ng SSH sa template ay hindi garantiya laban sa lockout. Ang validate-only mode ay nag-i-install pa rin ng mga package at nagsasagawa ng mga gawain sa konfigurasyon/serbisyo; hindi ito isang dry run na walang side effect.

## Mabilisang pagsisimula

I-build at i-install ang koleksiyon mula sa checkout na ito:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Ang pangalan ng file ay sumusunod sa bersyon sa [galaxy.yml](../../../galaxy.yml). Gamitin ang pangalan ng file na nalikha ng build kung magbago ang bersyong iyon.

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

Suriin ang nalikhang patakaran sa isang test host bago ito ilapat sa mga remote host. Magpanatili ng isang independiyenteng paraan ng pagbawi kapag nagbabago ng patakaran ng firewall.

## Pag-develop

Inilalarawan ng [CI workflow](../../../.github/workflows/ci.yml) at [mga senaryo ng Molecule](../../../molecule) ang naka-configure na pagpapatunay. Ang pagkakaroon ng mga ito ay hindi nagpapatunay ng matagumpay na deployment sa bawat distribusyon.

## Pagsunod at lisensya

Ang mas naunang README ay tumutukoy sa isang dokumento ng pagmamapa ng pagsunod na wala sa checkout na ito. Walang inaangking sertipikasyon o pagsunod sa framework dito. Ginagamit ng koleksiyon ang [lisensyang MIT](../../../LICENSE); itinatala ng [galaxy.yml](../../../galaxy.yml) ang metadata ng package.

## Suporta sa operating system

Sinubukan ang backend ng nftables gamit ang Ansible core 2.21 sa Ubuntu 22.04,
24.04 at 26.04 LTS, Debian 12 at 13, at Rocky Linux 9 at 10. Gumamit ng
pinapanatiling controller na may core 2.20 o mas bago para sa Python 3.14 ng Ubuntu 26.04.

Sinusuri ng Molecule ang convergence, ang pangalawang pagtakbo nang walang pagbabago, ang na-load na ruleset,
ang panuntunang nagpapahintulot sa SSH port 22, ang mga pahintulot sa konfigurasyon, at ang rollback pagkatapos ng isang inilagay na pagkabigo sa paglalapat.
Gumagamit ang Enterprise Linux ng `/etc/sysconfig/nftables.conf`; gumagamit ang Debian at
Ubuntu ng `/etc/nftables.conf`. Nananatiling nako-configure ang `nftables_conf_path`.
I-configure ang native na serbisyo ng nftables upang i-load ang anumang custom na path bago gamitin
ang override na iyon: nire-restart ng role ang serbisyo, na kung hindi ay maglo-load ng stock
file nito at maaaring pumalit sa nilalayong ruleset.

Sa Enterprise Linux, pinipigilan ng isang naiibang legacy na `/etc/nftables.conf` ang role
bago ang promotion. I-backup ang parehong file, pagkatapos ay ilipat ang napatunayang nakaraang
konfigurasyon sa native na path ng serbisyo. Bilang alternatibo, tahasang panatilihin
ang legacy na path at i-configure ang serbisyo nang naaayon. Huwag mag-iwan ng lumang
kopya sa `/etc/nftables.conf`: ang naiibang file doon ay magti-trigger ng guard sa
mga susunod na pagtakbo. Nananatiling magagamit ang mga nakaraang panuntunan sa napiling path bilang
pinagmulan ng rollback sa panahon ng upgrade.

Sinusubukan ng Rocky Linux ang path ng package at serbisyo ng Enterprise Linux. Ang mga
container test na ito ay hindi nagbibigay ng sertipikasyon sa mga subscription ng RHEL o sa isang production kernel.
Ang Ubuntu 20.04 na walang extended support, Debian 11, at ang mas lumang Python ng EL8 ay
wala sa kasalukuyang test matrix.
