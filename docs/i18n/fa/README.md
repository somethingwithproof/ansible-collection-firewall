<!-- translations:start -->
**Languages:** [English](../../../README.md) · **فارسی** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · **فارسی** · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

مجموعه‌ای از Ansible برای توصیف سیاست فایروال میزبان و اعمال پیکربندی nftables. گروه‌های آدرس، سیاست را از مجموعه قوانین تولیدشده جدا نگه می‌دارند.

## پیاده‌سازی و مرزها

- نقش [firewall](../../../roles/firewall/tasks/main.yml) مسئول مقادیر پیش‌فرض عمومی و اعتبارسنجی بک‌اند است؛ نقش [policy](../../../roles/policy/tasks/main.yml) نقطه ورود قدیمی را با وارد کردن آن حفظ می‌کند.
- نقش [nftables](../../../roles/nftables/tasks/main.yml) بسته nftables را نصب می‌کند، پیکربندی را آماده‌سازی می‌کند، آن را با `nft -c -f` اعتبارسنجی کرده و با `nft -f` اعمال می‌کند.
- بلوک اعمال (apply) در صورت بروز خطا تلاش می‌کند پیکربندی قبلی را، در صورت وجود، بازیابی کند. در دسترس بودن نسخه پشتیبان و دسترسی بازیابی همچنان از الزامات عملیاتی هستند.
- [قالب](../../../roles/nftables/templates/nftables.conf.j2) و [مقادیر پیش‌فرض](../../../roles/policy/defaults/main.yml) فیلدهای پشتیبانی‌شده سیاست را تعریف می‌کنند.

این مجموعه شامل نقش‌های `firewall`، `policy` و `nftables` است؛ تنها `nftables` یک بک‌اند فایروال را پیاده‌سازی می‌کند. نام بک‌اندهای iptables و firewalld در اعتبارسنجی سیاست پذیرفته می‌شوند، اما هیچ نقش بک‌اند متناظری در اینجا ارائه نشده است. وجود یک قانون SSH در قالب، تضمینی در برابر قفل شدن دسترسی نیست. حالت فقط‌اعتبارسنجی (validate-only) همچنان بسته‌ها را نصب می‌کند و وظایف پیکربندی/سرویس را انجام می‌دهد؛ این حالت یک اجرای آزمایشی بدون اثر جانبی (dry run) نیست.

## شروع سریع

مجموعه را از همین نسخه محلی بسازید و نصب کنید:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

نام فایل از نسخه موجود در [galaxy.yml](../../../galaxy.yml) پیروی می‌کند. در صورت تغییر آن نسخه، از نام فایلی که در فرایند ساخت تولید می‌شود استفاده کنید.

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

پیش از اعمال سیاست تولیدشده روی میزبان‌های از راه دور، آن را روی یک میزبان آزمایشی بررسی کنید. هنگام تغییر سیاست فایروال، یک مسیر بازیابی مستقل در اختیار داشته باشید.

## توسعه

[گردش‌کار CI](../../../.github/workflows/ci.yml) و [سناریوهای Molecule](../../../molecule) اعتبارسنجی پیکربندی‌شده را توصیف می‌کنند. وجود آن‌ها تضمین‌کننده استقرار موفق روی هر توزیعی نیست.

## انطباق و مجوز

نسخه پیشین README به یک سند نگاشت انطباق اشاره داشت که در این نسخه محلی وجود ندارد. در اینجا هیچ ادعایی درباره گواهینامه یا انطباق با چارچوب‌ها مطرح نمی‌شود. این مجموعه از [مجوز MIT](../../../LICENSE) استفاده می‌کند و فایل [galaxy.yml](../../../galaxy.yml) فراداده بسته را ثبت می‌کند.

## پشتیبانی از سیستم‌عامل

بک‌اند nftables با Ansible core 2.21 روی Ubuntu 22.04، 24.04 و 26.04 LTS، Debian 12 و 13 و Rocky Linux 9 و 10 آزمایش شده است. برای Python 3.14 در Ubuntu 26.04 از یک کنترلر تحت پشتیبانی با نسخه core 2.20 یا جدیدتر استفاده کنید.

Molecule همگرایی، اجرای دوم بدون تغییر، مجموعه قوانین بارگذاری‌شده، قانون مجازسازی پورت 22 برای SSH، مجوزهای فایل پیکربندی و بازگشت به حالت قبل پس از تزریق خطا در مرحله اعمال را بررسی می‌کند. Enterprise Linux از `/etc/sysconfig/nftables.conf` استفاده می‌کند؛ Debian و Ubuntu از `/etc/nftables.conf` استفاده می‌کنند. متغیر `nftables_conf_path` همچنان قابل پیکربندی است. پیش از استفاده از این بازنویسی مسیر، سرویس بومی nftables را برای بارگذاری مسیر سفارشی پیکربندی کنید: این نقش سرویس را راه‌اندازی مجدد می‌کند و در غیر این صورت فایل پیش‌فرض آن بارگذاری شده و ممکن است مجموعه قوانین مورد نظر را جایگزین کند.

در Enterprise Linux، وجود یک فایل قدیمی و متفاوت در `/etc/nftables.conf` پیش از نهایی‌سازی، اجرای نقش را متوقف می‌کند. از هر دو فایل نسخه پشتیبان بگیرید، سپس پیکربندی قبلی اعتبارسنجی‌شده را به مسیر سرویس بومی منتقل کنید. به‌عنوان جایگزین، مسیر قدیمی را به‌صراحت حفظ کرده و سرویس را متناسب با آن پیکربندی کنید. یک نسخه منسوخ در `/etc/nftables.conf` باقی نگذارید: وجود یک فایل متفاوت در آنجا در اجراهای بعدی محافظ ایمنی را فعال می‌کند. قوانین قبلی در مسیر انتخاب‌شده به‌عنوان منبع بازگشت به حالت قبل هنگام ارتقا در دسترس باقی می‌مانند.

Rocky Linux مسیر بسته و سرویس Enterprise Linux را آزمایش می‌کند. این آزمون‌های کانتینری به معنای تایید اشتراک‌های RHEL یا یک هسته (kernel) محیط عملیاتی نیستند. Ubuntu 20.04 بدون پشتیبانی توسعه‌یافته، Debian 11 و نسخه قدیمی‌تر Python در EL8 خارج از ماتریس آزمون فعلی قرار دارند.
