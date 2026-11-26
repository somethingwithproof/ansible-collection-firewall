<!-- translations:start -->
**Languages:** [English](../../../README.md) · **isiZulu** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · **isiZulu**
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Iqoqo le-Ansible lokuchaza inqubomgomo ye-firewall yomshini oyisikhungo (host) nokusebenzisa izilungiselelo ze-nftables. Amaqembu amakheli agcina inqubomgomo ihlukene neqoqo lemithetho elikhiqiziwe.

## Ukusebenza nemikhawulo

- I-[firewall](../../../roles/firewall/tasks/main.yml) iphethe izilungiselelo ezizenzakalelayo zomphakathi nokuqinisekiswa kwe-backend; i-[policy](../../../roles/policy/tasks/main.yml) igcina indawo yokungena yakudala ngokuyingenisa.
- I-[nftables](../../../roles/nftables/tasks/main.yml) ifaka i-nftables, ilungise izilungiselelo, iziqinisekise nge-`nft -c -f`, bese izisebenzisa nge-`nft -f`.
- **`nftables_backup: true`** — Isigaba sokusebenzisa sizama ukubuyisela izilungiselelo zangaphambilini ngemuva kokwehluleka uma bezikhona. Ukutholakala kwekhophi eyisipele nokufinyelela kokubuyisela kuhlala kuyizidingo zokusebenza.
- [Ithemphulethi](../../../roles/nftables/templates/nftables.conf.j2) kanye [nezilungiselelo ezizenzakalelayo](../../../roles/firewall/defaults/main.yml) kuchaza izinkambu zenqubomgomo ezisekelwayo.

Leli qoqo lihlanganisa `firewall`, `policy`, kanye ne-`nftables`; kuphela i-`nftables` esebenza njenge-backend ye-firewall. Amagama e-backend e-iptables ne-firewalld ayamukelwa ekuqinisekisweni kwenqubomgomo, kodwa azikho izindima (roles) ezihambisanayo ze-backend ezithunyelwa lapha. Umthetho we-SSH othemphulethini awusona isiqinisekiso sokuthi ngeke uvalelwe ngaphandle. Imodi yokuqinisekisa kuphela isafaka amaphakheji futhi yenze imisebenzi yokulungiselela/yesevisi; akusona isivivinyo sokulingisa esingenamiphumela eceleni.

## Ukuqala ngokushesha

Yakha bese ufaka iqoqo usuka kule khophi yekhodi:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Igama lefayela lilandela inguqulo eku-[galaxy.yml](../../../galaxy.yml). Sebenzisa igama lefayela elikhiqizwe ukwakhiwa uma leyo nguqulo ishintsha.

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
        office: ["203.0.113.0/24"]  # Faka inethiwekhi yehhovisi lakho esikhundleni salokhu (CIDR)
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

Buyekeza inqubomgomo ekhiqiziwe kumshini wokuhlola ngaphambi kokuyisebenzisa emishinini ekude. Gcina indlela ezimele yokubuyisela uma ushintsha inqubomgomo ye-firewall.

## Ukuthuthukiswa

[Uhlelo lomsebenzi lwe-CI](../../../.github/workflows/ci.yml) kanye [nezimo ze-Molecule](../../../molecule) kuchaza ukuqinisekiswa okuhleliwe. Ukuba khona kwakho akuqinisekisi ukuthunyelwa okuphumelelayo kulo lonke uhlobo lwesistimu (distribution).

## Ukuhambisana nemithetho nelayisense

I-README yangaphambilini yakhomba idokhumenti yokuhambisana nemithetho engekho kule khophi yekhodi. Akukho kuqinisekiswa kwesitifiketi noma ukuhambisana nohlaka okunikezwayo lapha. Iqoqo lisebenzisa [ilayisense ye-MIT](../../../LICENSE); i-[galaxy.yml](../../../galaxy.yml) irekhoda imethadatha yephakheji.

## Ukusekelwa kwamasistimu okusebenza

I-backend ye-nftables ihlolwa nge-Ansible core 2.21 ku-Ubuntu 22.04,
24.04 no-26.04 LTS, i-Debian 12 no-13, kanye ne-Rocky Linux 9 no-10. Sebenzisa
i-controller enakekelwayo ene-core 2.20 noma entsha nge-Python 3.14 ye-Ubuntu 26.04.

I-Molecule ihlola ukuhlangana kwezilungiselelo (convergence), ukusebenza kwesibili ngaphandle koshintsho, iqoqo lemithetho elilayishiwe,
umthetho wokuvumela ichweba 22 le-SSH, izimvume zezilungiselelo nokubuyisela emuva ngemuva kokwehluleka okufakiwe kokusebenzisa.
I-Enterprise Linux isebenzisa `/etc/sysconfig/nftables.conf`; i-Debian
ne-Ubuntu zisebenzisa `/etc/nftables.conf`. I-`nftables_conf_path` ihlala ilungiseka.
Lungiselela isevisi yomdabu ye-nftables ukuthi ilayishe noma iyiphi indlela eyenziwe ngokwezifiso ngaphambi kokusebenzisa
lokho kushintsha: indima (role) iqala kabusha isevisi, ebingalayisha ifayela layo
elivamile futhi ingase isuse iqoqo lemithetho elihlosiwe.

Ku-Enterprise Linux, i-`/etc/nftables.conf` yakudala ehlukile imisa indima (role)
ngaphambi kokukhushulelwa ekusebenzeni. Yenza ikhophi eyisipele yawo womabili amafayela, bese uhambisa izilungiselelo
zangaphambilini eziqinisekisiwe endleleni yesevisi yomdabu. Kungenjalo, gcina ngokucacile
indlela yakudala bese ulungiselela isevisi ngokufanele. Ungashiyi ikhophi endala
ku-`/etc/nftables.conf`: ifayela elihlukile lapho livusa isivikeli
ekusebenzeni okulandelayo. Imithetho yangaphambilini ihlala itholakala endleleni ekhethiwe njengomthombo
wokubuyisela emuva ngesikhathi sokuthuthukiswa kwenguqulo.

I-Rocky Linux ihlola iphakheji yendlela ye-Enterprise Linux nesevisi yayo. Lezi
zivivinyo zama-container aziqinisekisi okubhaliselwe kwe-RHEL noma i-kernel yokukhiqiza.
I-Ubuntu 20.04 engenakusekelwa okunwetshiwe, i-Debian 11 kanye ne-Python endala ye-EL8
zingaphandle kohlaka lokuhlola lwamanje.
