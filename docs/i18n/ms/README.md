<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Bahasa Melayu** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · **Bahasa Melayu** · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Koleksi Ansible untuk memerihalkan dasar tembok api hos dan menggunakan konfigurasi nftables. Kumpulan alamat memastikan dasar kekal terasing daripada set peraturan yang dijana.

## Pelaksanaan dan sempadan

- [firewall](../../../roles/firewall/tasks/main.yml) mengurus nilai lalai awam dan pengesahan bahagian belakang; [policy](../../../roles/policy/tasks/main.yml) mengekalkan titik masuk legasi dengan mengimportnya.
- [nftables](../../../roles/nftables/tasks/main.yml) memasang nftables, menyediakan konfigurasi sementara, mengesahkannya dengan `nft -c -f`, dan menerapkannya dengan `nft -f`.
- **`nftables_backup: true`** — Blok penerapan cuba memulihkan konfigurasi terdahulu selepas kegagalan apabila konfigurasi tersebut wujud. Ketersediaan sandaran dan akses pemulihan kekal menjadi keperluan operasi.
- [Templat](../../../roles/nftables/templates/nftables.conf.j2) dan [nilai lalai](../../../roles/firewall/defaults/main.yml) mentakrifkan medan dasar yang disokong.

Koleksi ini mempakejkan `firewall`, `policy`, dan `nftables`; hanya `nftables` yang melaksanakan bahagian belakang tembok api. Nama bahagian belakang untuk iptables dan firewalld diterima oleh pengesahan dasar, tetapi tiada peranan bahagian belakang yang sepadan disertakan di sini. Peraturan SSH dalam templat bukanlah jaminan terhadap keadaan terkunci keluar. Mod sahkan-sahaja masih memasang pakej dan melaksanakan tugas konfigurasi/perkhidmatan; ia bukanlah larian percubaan tanpa kesan sampingan.

## Mula pantas

Bina dan pasang koleksi daripada salinan semak keluar ini:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Nama fail mengikut versi dalam [galaxy.yml](../../../galaxy.yml). Gunakan nama fail yang dihasilkan oleh binaan sekiranya versi tersebut berubah.

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
        office: ["203.0.113.0/24"]  # Gantikan dengan rangkaian pejabat anda (CIDR)
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

Semak dasar yang dijana pada hos ujian sebelum menerapkannya pada hos jauh. Kekalkan laluan pemulihan yang berasingan apabila mengubah dasar tembok api.

## Pembangunan

[Aliran kerja CI](../../../.github/workflows/ci.yml) dan [senario Molecule](../../../molecule) memerihalkan pengesahan yang dikonfigurasikan. Kewujudannya tidak membuktikan pengerahan yang berjaya pada setiap pengedaran.

## Pematuhan dan lesen

README terdahulu merujuk kepada dokumen pemetaan pematuhan yang tidak terdapat dalam salinan semak keluar ini. Tiada pensijilan atau pematuhan rangka kerja dituntut di sini. Koleksi ini menggunakan [lesen MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) merekodkan metadata pakej.

## Sokongan sistem pengendalian

Bahagian belakang nftables diuji dengan Ansible core 2.21 pada Ubuntu 22.04, 24.04 dan 26.04 LTS, Debian 12 dan 13, serta Rocky Linux 9 dan 10. Gunakan pengawal yang diselenggara dengan core 2.20 atau lebih baharu untuk Python 3.14 pada Ubuntu 26.04.

Molecule menyemak penumpuan, larian kedua tanpa perubahan, set peraturan yang dimuatkan, peraturan membenarkan port 22 SSH, kebenaran konfigurasi dan gulung balik selepas kegagalan penerapan yang disuntik. Enterprise Linux menggunakan `/etc/sysconfig/nftables.conf`; Debian dan Ubuntu menggunakan `/etc/nftables.conf`. `nftables_conf_path` kekal boleh dikonfigurasikan. Konfigurasikan perkhidmatan nftables asli untuk memuatkan mana-mana laluan tersuai sebelum menggunakan penggantian tersebut: peranan ini memulakan semula perkhidmatan, yang jika sebaliknya akan memuatkan fail asalnya dan boleh menggantikan set peraturan yang dimaksudkan.

Pada Enterprise Linux, fail legasi `/etc/nftables.conf` yang berbeza akan menghentikan peranan sebelum penerapan. Buat sandaran bagi kedua-dua fail, kemudian alihkan konfigurasi terdahulu yang telah disahkan ke laluan perkhidmatan asli. Sebagai alternatif, kekalkan laluan legasi secara eksplisit dan konfigurasikan perkhidmatan dengan sewajarnya. Jangan tinggalkan salinan lapuk di `/etc/nftables.conf`: fail yang berbeza di situ akan mencetuskan semakan pelindung pada larian seterusnya. Peraturan terdahulu kekal tersedia di laluan yang dipilih sebagai sumber gulung balik semasa naik taraf.

Rocky Linux menguji laluan pakej dan perkhidmatan Enterprise Linux. Ujian kontena ini tidak memperakui langganan RHEL atau kernel pengeluaran. Ubuntu 20.04 tanpa sokongan lanjutan, Debian 11 dan Python versi lebih lama pada EL8 berada di luar matriks ujian semasa.
