<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Bahasa Indonesia** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · **Bahasa Indonesia**
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Koleksi Ansible untuk mendeskripsikan kebijakan firewall host dan menerapkan konfigurasi nftables. Grup alamat menjaga kebijakan tetap terpisah dari ruleset yang dihasilkan.

## Implementasi dan batasan

- [firewall](../../../roles/firewall/tasks/main.yml) mengelola nilai default publik dan validasi backend; [policy](../../../roles/policy/tasks/main.yml) mempertahankan titik masuk lama dengan mengimpornya.
- [nftables](../../../roles/nftables/tasks/main.yml) memasang nftables, menyiapkan konfigurasi sementara, memvalidasinya dengan `nft -c -f`, dan menerapkannya dengan `nft -f`.
- **`nftables_backup: true`** — Blok penerapan berupaya memulihkan konfigurasi sebelumnya setelah terjadi kegagalan jika konfigurasi tersebut tersedia. Ketersediaan cadangan dan akses pemulihan tetap menjadi persyaratan operasional.
- [templat](../../../roles/nftables/templates/nftables.conf.j2) dan [nilai default](../../../roles/firewall/defaults/main.yml) mendefinisikan kolom kebijakan yang didukung.

Koleksi ini memaketkan `firewall`, `policy`, dan `nftables`; hanya `nftables` yang mengimplementasikan backend firewall. Nama backend untuk iptables dan firewalld diterima oleh validasi kebijakan, tetapi tidak ada peran backend terkait yang disertakan di sini. Aturan SSH di dalam templat bukan jaminan untuk mencegah terkuncinya akses. Mode validasi saja tetap memasang paket dan menjalankan tugas konfigurasi/layanan; mode ini bukan dry run tanpa efek samping.

## Mulai cepat

Bangun dan pasang koleksi ini dari salinan repositori ini:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Nama berkas mengikuti versi di [galaxy.yml](../../../galaxy.yml). Gunakan nama berkas yang dihasilkan oleh proses build jika versi tersebut berubah.

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
        office: ["203.0.113.0/24"]  # Ganti dengan jaringan kantor Anda (CIDR)
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

Tinjau kebijakan yang dihasilkan pada host pengujian sebelum menerapkannya ke host jarak jauh. Pertahankan jalur pemulihan independen saat mengubah kebijakan firewall.

## Pengembangan

[Alur kerja CI](../../../.github/workflows/ci.yml) dan [skenario Molecule](../../../molecule) menjelaskan validasi yang dikonfigurasi. Keberadaannya tidak menjamin keberhasilan penerapan di setiap distribusi.

## Kepatuhan dan lisensi

README sebelumnya merujuk pada dokumen pemetaan kepatuhan yang tidak terdapat dalam salinan repositori ini. Tidak ada sertifikasi atau kepatuhan kerangka kerja yang diklaim di sini. Koleksi ini menggunakan [lisensi MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) mencatat metadata paket.

## Dukungan sistem operasi

Backend nftables diuji dengan Ansible core 2.21 pada Ubuntu 22.04,
24.04, dan 26.04 LTS, Debian 12 dan 13, serta Rocky Linux 9 dan 10. Gunakan
controller yang masih dipelihara dengan core 2.20 atau lebih baru untuk Python 3.14 pada Ubuntu 26.04.

Molecule memeriksa konvergensi, eksekusi kedua tanpa perubahan, ruleset yang dimuat,
aturan izin port 22 SSH, izin berkas konfigurasi, dan rollback setelah kegagalan penerapan
yang disuntikkan. Enterprise Linux menggunakan `/etc/sysconfig/nftables.conf`; Debian dan
Ubuntu menggunakan `/etc/nftables.conf`. `nftables_conf_path` tetap dapat dikonfigurasi.
Konfigurasikan layanan nftables bawaan agar memuat jalur kustom apa pun sebelum menggunakan
penimpaan tersebut: peran ini memulai ulang layanan, yang jika tidak dikonfigurasi akan memuat
berkas bawaannya dan dapat menggantikan ruleset yang dimaksud.

Pada Enterprise Linux, berkas lama `/etc/nftables.conf` yang berbeda akan menghentikan peran
sebelum promosi konfigurasi. Cadangkan kedua berkas, lalu pindahkan konfigurasi sebelumnya
yang telah divalidasi ke jalur layanan bawaan. Sebagai alternatif, pertahankan jalur lama
secara eksplisit dan konfigurasikan layanan sesuai dengan itu. Jangan tinggalkan salinan
usang di `/etc/nftables.conf`: berkas yang berbeda di sana akan memicu pengaman pada
eksekusi berikutnya. Aturan sebelumnya tetap tersedia di jalur yang dipilih sebagai sumber
rollback saat pemutakhiran.

Rocky Linux menguji jalur paket dan layanan Enterprise Linux. Pengujian kontainer
ini tidak mensertifikasi langganan RHEL atau kernel produksi.
Ubuntu 20.04 tanpa dukungan diperpanjang, Debian 11, dan Python lama pada EL8
berada di luar matriks pengujian saat ini.
