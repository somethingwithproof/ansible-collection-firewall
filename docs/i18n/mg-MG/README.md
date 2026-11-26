<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Malagasy** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · **Malagasy** · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Angona Ansible natao hamaritana ny politikan'ny aro-afo (firewall) amin'ny mpampiantrano sy hampiharana ny fandrindrana nftables. Ny vondron'adiresy dia mitazona ny politika ho misaraka amin'ny andian-fitsipika vokarina.

## Fampiharana sy fetra

- Ny [firewall](../../../roles/firewall/tasks/main.yml) no mitantana ny soatoavina fototra ho an'ny daholobe sy ny fanamarinana ny backend; ny [policy](../../../roles/policy/tasks/main.yml) dia mitahiry ny teboka fidirana teo aloha amin'ny alalan'ny fanafarana azy.
- Ny [nftables](../../../roles/nftables/tasks/main.yml) dia mametraka ny nftables, manomana ny fandrindrana, manamarina azy amin'ny `nft -c -f`, ary mampihatra azy amin'ny `nft -f`.
- Ny vondron'asa fampiharana dia manandrana mamerina ny fandrindrana teo aloha rehefa misy tsy fahombiazana raha nisy izany. Ny fisian'ny tahiry fiarovana (backup) sy ny fidirana fanarenana dia mijanona ho fepetra ilaina amin'ny fampiasana.
- Ny [modely](../../../roles/nftables/templates/nftables.conf.j2) sy ny [soatoavina fototra](../../../roles/policy/defaults/main.yml) no mamaritra ny sahan-politika tohanana.

Misy anjara asa `firewall`, `policy`, ary `nftables` ao anatin'ity angona ity; ny `nftables` ihany no mampihatra backend aro-afo. Eken'ny fanamarinana politika ny anaran'ny backend ho an'ny iptables sy firewalld, saingy tsy misy anjara asa backend mifanaraka amin'izany aterina ato. Ny fitsipika SSH ao amin'ny modely dia tsy antoka fa tsy ho voahidy ivelany ianao. Ny fomba fanamarinana fotsiny (validate-only) dia mbola mametraka fonosana sy manatanteraka asam-pandrindrana/tolotra; tsy fitsapana tsy misy fiantraikany (dry run) izany.

## Fanombohana haingana

Amboary ary apetraho ny angona avy amin'ity dika nalaina ity:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Manaraka ny dika ao amin'ny [galaxy.yml](../../../galaxy.yml) ny anaran'ny rakitra. Ampiasao ny anaran'ny rakitra vokarin'ny fanamboarana raha miova izany dika izany.

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

Hamarino aloha ny politika vokarina eo amin'ny mpampiantrano fitsapana alohan'ny hampiharana azy amin'ny mpampiantrano lavitra. Mitazona lalana fanarenana mahaleo tena hatrany rehefa manova politikan'ny aro-afo.

## Fampandrosoana

Ny [fizotran'asa CI](../../../.github/workflows/ci.yml) sy ny [sehatra fitsapana Molecule](../../../molecule) no mamaritra ny fanamarinana voarindra. Ny fisian'izy ireo dia tsy manaporofo fametrahana mahomby amin'ny fizarana Linux rehetra.

## Fanarahan-dalàna sy fahazoan-dalana

Ny README teo aloha dia nanondro antontan-taratasy fampifandraisana fanarahan-dalàna izay tsy hita ao amin'ity dika nalaina ity. Tsy misy fanamarinana na fanarahana rafitra fenitra lazaina eto. Mampiasa ny [fahazoan-dalana MIT](../../../LICENSE) ity angona ity; ny [galaxy.yml](../../../galaxy.yml) no mitahiry ny metadata-n'ny fonosana.

## Fanohanana rafitra fiasana

Voatsapa amin'ny Ansible core 2.21 ny backend nftables amin'ny Ubuntu 22.04, 24.04 ary 26.04 LTS, Debian 12 sy 13, ary Rocky Linux 9 sy 10. Mampiasà mpibaiko (controller) voakojakoja misy core 2.20 na vaovao kokoa ho an'ny Python 3.14 an'ny Ubuntu 26.04.

Manamarina ny fampifanarahana (convergence), fampandehanana fanindroany tsy misy fiovana, ny andian-fitsipika tafiditra, ny fitsipika mamela ny seranana SSH 22, ny alalan'ny rakitra fandrindrana, ary ny fiverenana amin'ny teo aloha (rollback) aorian'ny tsy fahombiazana nampidirina niniana natao ny Molecule. Ny Enterprise Linux dia mampiasa `/etc/sysconfig/nftables.conf`; ny Debian sy Ubuntu dia mampiasa `/etc/nftables.conf`. Azo ovaina hatrany ny `nftables_conf_path`. Rindrio aloha ny tolotra nftables voajanahary mba hampiditra ny lalana namboarina alohan'ny hampiasana izany fanovana izany: averin'ny anjara asa velomina ny tolotra, ka raha tsy izany dia hampiditra ny rakitra fototra izy ary mety hanolo ny andian-fitsipika nokasaina.

Amin'ny Enterprise Linux, ny rakitra `/etc/nftables.conf` taloha izay samy hafa dia mampiato ny anjara asa alohan'ny fampiharana. Manaova tahiry fiarovana (backup) amin'ireo rakitra roa, avy eo afindrao any amin'ny lalan'ny tolotra voajanahary ilay fandrindrana teo aloha voamarina. Raha tsy izany, tazony mazava ny lalana taloha ary rindrio araka izany ny tolotra. Aza mamela dika lany andro ao amin'ny `/etc/nftables.conf`: ny rakitra samy hafa ao dia manetsika ny fiarovana amin'ny fampandehanana manaraka. Mijanona ho azo ampiasaina eo amin'ny lalana voafidy ny fitsipika teo aloha ho loharanon'ny fiverenana amin'ny teo aloha mandritra ny fanavaozana.

Ny Rocky Linux dia mitsapa ny fonosana sy ny lalan'ny tolotra Enterprise Linux. Ireo fitsapana amin'ny kaontenera ireo dia tsy manamarina ny famandrihana RHEL na ny kernel famokarana. Ny Ubuntu 20.04 tsy misy fanohanana maharitra, ny Debian 11, ary ny Python taloha an'ny EL8 dia ivelan'ny lasitra fitsapana ankehitriny.
