<!-- translations:start -->
**Languages:** [English](../../../README.md) · **isiXhosa** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · **isiXhosa** · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Ingqokelela ye-Ansible yokuchaza umgaqo-nkqubo we-firewall yomncedisi kunye nokusebenzisa uqwalaselo lwe-nftables. Amaqela eedilesi agcina umgaqo-nkqubo uhlukene noluhlu lwemithetho olwenziweyo.

## Ukumiselwa nemida

- I-[firewall](../../../roles/firewall/tasks/main.yml) ilawula amaxabiso asisiseko esidlangalaleni kunye nokuqinisekiswa kwe-backend; i-[policy](../../../roles/policy/tasks/main.yml) igcina indawo yokungena yakudala ngokuyingenisa.
- I-[nftables](../../../roles/nftables/tasks/main.yml) ifaka i-nftables, ilungiselela uqwalaselo, iluqinisekisa nge-`nft -c -f`, kwaye ilusebenzisa nge-`nft -f`.
- Icandelo lokusebenzisa uqwalaselo lizama ukubuyisela uqwalaselo lwangaphambili emva kokusilela ukuba belukhona. Ukufumaneka kwekope yokugcina kunye nofikelelo lokubuyisela kuhlala kuyimfuneko yokusebenza.
- [Itemplate](../../../roles/nftables/templates/nftables.conf.j2) kunye [namaxabiso asisiseko](../../../roles/policy/defaults/main.yml) zichaza imihlaba yomgaqo-nkqubo exhaswayo.

Le ngqokelela iquka i-`firewall`, i-`policy`, kunye ne-`nftables`; kuphela yi-`nftables` ephumeza i-backend ye-firewall. Amagama e-backend e-iptables kunye ne-firewalld ayamkelwa kuqinisekiso lomgaqo-nkqubo, kodwa akukho zindima ze-backend ezihambelana nazo ezithunyelwa apha. Umthetho we-SSH okwitemplate awusoze ube sisiqinisekiso sokuthintela ukuvalelwa ngaphandle. Imo yokuqinisekisa kuphela isafaka iipakethe kwaye yenze imisebenzi yoqwalaselo neenkonzo; ayisiyiyo imeko yovavanyo engenaziphumo zotshintsho.

## Ukuqalisa ngokukhawuleza

Yakha uze ufake ingqokelela ukusuka kule kope yekhowudi:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Igama lefayile lilandela inguqulelo ekwi-[galaxy.yml](../../../galaxy.yml). Sebenzisa igama lefayile eliveliswe kulwakhiwo ukuba loo nguqulelo iyatshintsha.

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

Phonononga umgaqo-nkqubo ovelisiweyo kumncedisi wovavanyo phambi kokuwusebenzisa kubancedisi abakude. Gcina indlela ezimeleyo yokubuyisela xa utshintsha umgaqo-nkqubo we-firewall.

## Uphuhliso

[Inkqubo yokusebenza ye-CI](../../../.github/workflows/ci.yml) kunye [neemeko zovavanyo ze-Molecule](../../../molecule) zichaza uqinisekiso oluqwalaselweyo. Ubukho bazo abuqinisekisi ukufakelwa okunempumelelo kulo lonke uhlobo lwenkqubo yokusebenza.

## Ukuthobela imithetho nelayisenisi

I-README yangaphambili yayibhekisela kuxwebhu lolungelelwaniso lokuthobela imithetho olungekho kule kope yekhowudi. Akukho siqinisekiso okanye ukuthobela isakhelo esibangwayo apha. Le ngqokelela isebenzisa [ilayisenisi ye-MIT](../../../LICENSE); i-[galaxy.yml](../../../galaxy.yml) igcina imetadata yephakheji.

## Inkxaso yeenkqubo zokusebenza

I-backend ye-nftables ivavanywe nge-Ansible core 2.21 kwi-Ubuntu 22.04,
24.04 kunye ne-26.04 LTS, i-Debian 12 kunye ne-13, kunye ne-Rocky Linux 9 kunye ne-10. Sebenzisa
isilawuli esigcinwayo esine-core 2.20 okanye entsha kwi-Python 3.14 ye-Ubuntu 26.04.

I-Molecule ihlola ukudibana koqwalaselo, ukusebenza okwesibini ngaphandle kotshintsho, uluhlu lwemithetho olulayishiweyo,
umthetho ovumela i-SSH kwizibuko 22, iimvume zoqwalaselo kunye nokubuyisela emva kokusilela kokusebenzisa okufakelweyo.
I-Enterprise Linux isebenzisa `/etc/sysconfig/nftables.conf`; i-Debian
ne-Ubuntu zisebenzisa `/etc/nftables.conf`. I-`nftables_conf_path` ihlala inokuqwalaselwa.
Qwalasela inkonzo yemveli ye-nftables ukuba ilayishe nayiphi na indlela elungiselelweyo phambi kokusebenzisa
olo tshintsho: indima iqalisa kwakhona inkonzo, nto leyo engathi ngenye indlela ilayishe ifayile yayo
esisiseko ize ithathe indawo yoluhlu lwemithetho ekujoliswe kulo.

Kwi-Enterprise Linux, ifayile yakudala eyahlukileyo `/etc/nftables.conf` imisa indima
phambi kokuba uqwalaselo luphunyezwe. Gcina iikopi zokhuseleko zazo zombini ezi fayile, uze uhambise
uqwalaselo lwangaphambili oluqinisekisiweyo kwindlela yenkonzo yemveli. Kungenjalo, gcina ngokucacileyo
indlela yakudala uze uqwalasele inkonzo ngokuhambelana nayo. Musa ukushiya ikopi endala
kwi-`/etc/nftables.conf`: ifayile eyahlukileyo apho ibangela isikhuseli kwiziganeko zokusebenza
ezilandelayo. Imithetho yangaphambili ihlala ifumaneka kwindlela ekhethiweyo njengomthombo
wokubuyisela ngexesha lohlaziyo.

I-Rocky Linux ivavanya indlela yepakethe neyenkonzo ye-Enterprise Linux. Olu
vavanyo lwezikhongozeli aluqinisekisi imirhumo ye-RHEL okanye i-kernel yokuvelisa.
I-Ubuntu 20.04 engenankxaso eyandisiweyo, i-Debian 11 kunye ne-Python endala ye-EL8
zingaphandle kuluhlu lwangoku lovavanyo.
