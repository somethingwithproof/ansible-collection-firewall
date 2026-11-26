<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Hausa** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · **Hausa** · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Tarin Ansible don bayyana manufar firewall ta na'ura da aiwatar da saitin nftables. Rukunonin adireshi suna ware manufar daban da jerin ƙa'idojin da aka samar.

## Aiwatarwa da iyakoki

- [firewall](../../../roles/firewall/tasks/main.yml) ne ke da alhakin saitunan tsoho na jama'a da tantance backend; [policy](../../../roles/policy/tasks/main.yml) yana kiyaye tsohuwar hanyar shiga ta hanyar shigo da shi.
- [nftables](../../../roles/nftables/tasks/main.yml) yana girka nftables, ya shirya saitin, ya tantance shi da `nft -c -f`, sannan ya aiwatar da shi da `nft -f`.
- Sashen aiwatarwa yana ƙoƙarin mayar da tsohon saiti bayan gazawa idan akwai shi a baya. Samun ajiyar baya (backup) da hanyar dawo da samun dama sun ci gaba da zama buƙatun aiki.
- [Samfuri](../../../roles/nftables/templates/nftables.conf.j2) da [saitunan tsoho](../../../roles/policy/defaults/main.yml) suna bayyana filayen manufar da ake goyon baya.

Tarin ya ƙunshi `firewall`, `policy`, da `nftables`; `nftables` kaɗai ne ke aiwatar da backend na firewall. Tantancewar manufa tana karɓar sunayen backend na iptables da firewalld, amma babu ayyukan (roles) backend masu dacewa da aka haɗa a nan. Ƙa'idar SSH a cikin samfurin ba tabbaci ba ce cewa ba za a kulle hanyar shiga ba. Yanayin tantancewa kaɗai yana ci gaba da girka fakiti tare da gudanar da ayyukan saiti da sabis; ba gwajin bushe (dry run) ba ne marar tasiri.

## Fara aiki da sauri

Gina kuma girka tarin daga wannan kwafin ma'ajiyar:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Sunan fayil ɗin yana bin sigar da ke cikin [galaxy.yml](../../../galaxy.yml). Yi amfani da sunan fayil ɗin da ginin ya samar idan wannan sigar ta canza.

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

Duba manufar da aka samar a kan na'urar gwaji kafin aiwatar da ita a kan na'urori na nesa. Ajiye wata hanyar farfaɗowa mai zaman kanta yayin canza manufar firewall.

## Haɓakawa

[Tsarin aikin CI](../../../.github/workflows/ci.yml) da [yanayin gwajin Molecule](../../../molecule) suna bayyana tantancewar da aka saita. Kasancewarsu ba ya tabbatar da nasarar turawa a kan kowane nau'in tsarin aiki.

## Bin ƙa'idoji da lasisi

Tsohon README ya ambaci wani kundin taswirar bin ƙa'idoji wanda babu shi a cikin wannan kwafin ma'ajiyar. Ba a da'awar wata takardar shaida ko bin wani tsarin ƙa'idoji a nan. Tarin yana amfani da [lasisin MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) yana adana bayanan metadata na fakitin.

## Goyon bayan tsarin aiki

An gwada backend na nftables tare da Ansible core 2.21 a kan Ubuntu 22.04,
24.04 da 26.04 LTS, Debian 12 da 13, da kuma Rocky Linux 9 da 10. Yi amfani da
mai sarrafawa (controller) da ake kula da shi mai core 2.20 ko sabo don Python 3.14 na Ubuntu 26.04.

Molecule yana bincika daidaituwa (convergence), gudu na biyu ba tare da canje-canje ba, jerin ƙa'idojin da aka ɗora,
ƙa'idar ba da izinin tashar SSH 22, izinin fayilolin saiti da mayar da baya (rollback) bayan gazawar aiwatarwa
da aka sanya da gangan. Enterprise Linux yana amfani da `/etc/sysconfig/nftables.conf`; Debian da
Ubuntu suna amfani da `/etc/nftables.conf`. Ana iya ci gaba da saita `nftables_conf_path`.
Saita sabis ɗin nftables na asali don ɗora kowace hanya ta musamman kafin amfani da
wannan canjin: aikin (role) yana sake kunna sabis ɗin, wanda in ba haka ba zai ɗora fayil ɗinsa na asali
kuma yana iya maye gurbin jerin ƙa'idojin da aka yi niyya.

A kan Enterprise Linux, tsohon fayil ɗin `/etc/nftables.conf` mai bambanci yana tsayar da aikin
kafin ɗaukaka saitin. Yi ajiyar baya ga duka fayilolin biyu, sannan ka matsar da tsohon saitin
da aka tantance zuwa hanyar sabis ta asali. A madadin haka, a fili ka riƙe
tsohuwar hanyar kuma ka saita sabis ɗin daidai da haka. Kada ka bar tsohon
kwafi a `/etc/nftables.conf`: fayil mai bambanci a wurin zai kunna kariyar tsaro a
gudu na gaba. Tsofaffin ƙa'idojin suna ci gaba da kasancewa a hanyar da aka zaɓa a matsayin
tushen mayar da baya yayin haɓakawa.

Rocky Linux yana gwada hanyar fakiti da sabis ta Enterprise Linux. Waɗannan
gwaje-gwajen kwantena ba sa tabbatar da biyan kuɗin RHEL ko kernel na aikin zahiri.
Ubuntu 20.04 marar tsawaitaccen goyon baya, Debian 11 da tsohon Python na EL8 suna
wajen tsarin gwaji na yanzu.
