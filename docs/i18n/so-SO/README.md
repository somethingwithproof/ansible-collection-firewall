<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Soomaali** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · **Soomaali** · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Ururinta Ansible ee lagu qeexo siyaasadda derbiga-dabka (firewall) ee martigeliyaha laguna hirgeliyo habaynta nftables. Kooxaha cinwaannadu waxay siyaasadda ka soocaan xeerarka la soo saaray.

## Hirgelinta iyo xuduudaha

- [firewall](../../../roles/firewall/tasks/main.yml) wuxuu maamulaa qiimayaasha caadiga ah ee dadweynaha iyo xaqiijinta nidaamka dambe (backend); [policy](../../../roles/policy/tasks/main.yml) wuxuu ilaaliyaa barta gelitaanka ee hore isagoo soo dejinaya.
- [nftables](../../../roles/nftables/tasks/main.yml) wuxuu rakibaa nftables, diyaariyaa habaynta, ku xaqiijiyaa `nft -c -f`, wuxuuna ku hirgeliyaa `nft -f`.
- **`nftables_backup: true`** — Qaybta hirgelintu waxay isku daydaa inay soo celiso habayntii hore haddii fashil dhaco marka mid hore jirtay. Helitaanka nuqul-kaydinta iyo marinka soo-kabashada waxay sii ahaanayaan shuruudo hawlgal.
- [Qaab-dhismeedka](../../../roles/nftables/templates/nftables.conf.j2) iyo [qiimayaasha caadiga ah](../../../roles/firewall/defaults/main.yml) waxay qeexaan meelaha siyaasadda ee la taageero.

Ururintu waxay xirmaysaa `firewall`, `policy`, iyo `nftables`; kaliya `nftables` ayaa hirgeliya nidaamka dambe ee derbiga-dabka. Magacyada nidaamka dambe ee iptables iyo firewalld waa la aqbalaa marka la xaqiijinayo siyaasadda, laakiin wax doorar nidaam dambe ah oo u dhigma laguma soo darin halkan. Xeerka SSH ee ku jira qaab-dhismeedku ma aha dammaanad ka dhan ah xidhitaanka gelitaanka. Habka xaqiijinta-kaliya weli wuxuu rakibaa xirmooyinka wuxuuna fuliyaa hawlaha habaynta iyo adeegga; ma aha tijaabo qallalan oo aan wax saamayn ah lahayn.

## Bilow degdeg ah

Ka dhis oo ka rakib ururinta nuqulkan kaydka:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Magaca faylku wuxuu raacaa nooca ku jira [galaxy.yml](../../../galaxy.yml). Isticmaal magaca faylka uu dhismuhu soo saaro haddii noocaasi isbeddelo.

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
        office: ["203.0.113.0/24"]  # Ku beddel shabakadda xafiiskaaga (CIDR)
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

Dib u eeg siyaasadda la soo saaray adigoo isticmaalaya martigeliye tijaabo ah ka hor inta aanad ku hirgelin martigeliyeyaasha fog. Hayso marin soo-kabasho oo madax-bannaan marka aad beddelayso siyaasadda derbiga-dabka.

## Horumarinta

[Socodka shaqada CI](../../../.github/workflows/ci.yml) iyo [xaaladaha Molecule](../../../molecule) waxay qeexaan xaqiijinta la habeeyey. Jiritaankoodu ma caddeynayo in gayntu ku guulaysanayso qaybin kasta oo Linux ah.

## U hoggaansanaanta iyo shatiga

README-gii hore wuxuu tixraacay dukumeenti khariidaynta u-hoggaansanaanta ah oo aan ku jirin nuqulkan. Wax shahaado ah ama u-hoggaansanaan qaab-dhismeed laguma sheegto halkan. Ururintu waxay adeegsataa [shatiga MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) wuxuu diiwaangeliyaa xogta-hoose (metadata) ee xirmada.

## Taageerada nidaamka hawlgalka

Nidaamka dambe ee nftables waxaa lagu tijaabiyay Ansible core 2.21 oo ku shaqeeya Ubuntu 22.04, 24.04 iyo 26.04 LTS, Debian 12 iyo 13, iyo Rocky Linux 9 iyo 10. Isticmaal koontaroolaha la dayactiro ee leh core 2.20 ama ka cusub si loogu isticmaalo Python 3.14 ee Ubuntu 26.04.

Molecule wuxuu hubiyaa iswaafaqsanaanta, socodsiin labaad oo aan isbeddel lahayn, xeerarka la raray, xeer oggolaanaya dekedda SSH 22, oggolaanshaha faylka habaynta iyo dib-u-celinta ka dib fashil hirgelin oo si ula kac ah loo geliyay. Enterprise Linux wuxuu isticmaalaa `/etc/sysconfig/nftables.conf`; Debian iyo Ubuntu waxay isticmaalaan `/etc/nftables.conf`. `nftables_conf_path` weli waa la habayn karaa. U habeey adeegga asalka ah ee nftables inuu raro marin kasta oo gaar ah ka hor inta aanad adeegsan beddelkaas: doorku wuxuu dib u bilaabaa adeegga, kaas oo haddii kale raraya faylkiisa caadiga ah isla markaana beddeli kara xeerarka la doonayay.

Enterprise Linux dhexdeeda, fayl hore oo kala duwan oo ku yaal `/etc/nftables.conf` wuxuu joojiyaa doorka ka hor inta aan habaynta la hawlgelin. Labada faylba nuqul-kaydi, ka dibna u wareeji habayntii hore ee la xaqiijiyay marinka adeegga asalka ah. Haddii kale, si cad u hayso marinka hore oo adeegga u habeey si waafaqsan. Ha kaga tegin nuqul duugoobay `/etc/nftables.conf`: fayl kala duwan oo halkaas yaalla wuxuu kiciyaa ilaalinta socodsiinnada dambe. Xeerarkii hore waxay sii ahaanayaan kuwo laga heli karo marinka la doortay iyagoo ah isha dib-u-celinta inta lagu jiro cusboonaysiinta.

Rocky Linux wuxuu tijaabiyaa xirmada iyo marinka adeegga ee Enterprise Linux. Tijaabooyinkan koonteenarrada ma caddeynayaan rukhsadaha RHEL ama kernel-ka wax-soo-saarka. Ubuntu 20.04 oo aan lahayn taageero dheeraad ah, Debian 11 iyo Python-ka duugga ah ee EL8 waxay ka baxsan yihiin shaxda tijaabada ee hadda.
