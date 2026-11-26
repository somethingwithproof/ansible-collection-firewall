<!-- translations:start -->
**Languages:** [English](../../../README.md) · **اردو** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · **اردو** · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

ہوسٹ فائر وال پالیسی بیان کرنے اور nftables کنفیگریشن لاگو کرنے کے لیے ایک Ansible کلیکشن۔ ایڈریس گروپس پالیسی کو تیار کردہ رول سیٹ سے الگ رکھتے ہیں۔

## نفاذ اور حدود

- [firewall](../../../roles/firewall/tasks/main.yml) عوامی ڈیفالٹس اور بیک اینڈ کی توثیق کا ذمہ دار ہے؛ [policy](../../../roles/policy/tasks/main.yml) اسے امپورٹ کر کے پرانے انٹری پوائنٹ کو برقرار رکھتا ہے۔
- [nftables](../../../roles/nftables/tasks/main.yml) nftables انسٹال کرتا ہے، کنفیگریشن کو اسٹیج کرتا ہے، `nft -c -f` سے اس کی توثیق کرتا ہے، اور `nft -f` سے اسے لاگو کرتا ہے۔
- **`nftables_backup: true`** — لاگو کرنے والا بلاک ناکامی کی صورت میں، اگر پچھلی کنفیگریشن موجود ہو تو اسے بحال کرنے کی کوشش کرتا ہے۔ بیک اپ کی دستیابی اور ریکوری رسائی بدستور آپریشنل تقاضے ہیں۔
- [ٹیمپلیٹ](../../../roles/nftables/templates/nftables.conf.j2) اور [ڈیفالٹس](../../../roles/firewall/defaults/main.yml) سپورٹ شدہ پالیسی فیلڈز کی وضاحت کرتے ہیں۔

یہ کلیکشن `firewall`، `policy`، اور `nftables` کو پیکیج کرتا ہے؛ صرف `nftables` فائر وال بیک اینڈ کا نفاذ کرتا ہے۔ پالیسی کی توثیق میں iptables اور firewalld کے بیک اینڈ نام قبول کیے جاتے ہیں، لیکن یہاں ان کے متعلقہ بیک اینڈ رولز فراہم نہیں کیے گئے ہیں۔ ٹیمپلیٹ میں موجود SSH رول لاک آؤٹ سے بچاؤ کی ضمانت نہیں دیتا۔ صرف توثیق (validate-only) موڈ میں بھی پیکیجز انسٹال ہوتے ہیں اور کنفیگریشن/سروس ٹاسکس انجام پاتے ہیں؛ یہ ضمنی اثرات سے پاک ڈرائی رن نہیں ہے۔

## فوری آغاز

اس چیک آؤٹ سے کلیکشن کو بلڈ اور انسٹال کریں:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

فائل کا نام [galaxy.yml](../../../galaxy.yml) میں موجود ورژن کے مطابق ہوتا ہے۔ اگر وہ ورژن تبدیل ہو جائے تو بلڈ کے نتیجے میں بننے والی فائل کا نام استعمال کریں۔

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
        office: ["203.0.113.0/24"]  # اسے اپنے دفتر کے نیٹ ورک سے بدلیں (CIDR)
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

ریموٹ ہوسٹس پر لاگو کرنے سے پہلے کسی ٹیسٹ ہوسٹ پر تیار کردہ پالیسی کا جائزہ لیں۔ فائر وال پالیسی تبدیل کرتے وقت ایک آزاد ریکوری راستہ برقرار رکھیں۔

## ڈیولپمنٹ

[CI ورک فلو](../../../.github/workflows/ci.yml) اور [Molecule منظرنامے](../../../molecule) کنفیگر شدہ توثیق کی وضاحت کرتے ہیں۔ ان کی موجودگی ہر ڈسٹری بیوشن پر کامیاب تعیناتی کی تصدیق نہیں کرتی۔

## تعمیل اور لائسنس

سابقہ README میں ایک تعمیل-میپنگ (compliance-mapping) دستاویز کا حوالہ دیا گیا تھا جو اس چیک آؤٹ میں موجود نہیں ہے۔ یہاں کسی سرٹیفیکیشن یا فریم ورک کی تعمیل کا دعویٰ نہیں کیا گیا ہے۔ یہ کلیکشن [MIT لائسنس](../../../LICENSE) استعمال کرتا ہے؛ [galaxy.yml](../../../galaxy.yml) پیکیج میٹا ڈیٹا ریکارڈ کرتا ہے۔

## آپریٹنگ سسٹم سپورٹ

nftables بیک اینڈ کو Ansible core 2.21 کے ساتھ Ubuntu 22.04، 24.04 اور 26.04 LTS، Debian 12 اور 13، اور Rocky Linux 9 اور 10 پر ٹیسٹ کیا گیا ہے۔ Ubuntu 26.04 کے Python 3.14 کے لیے core 2.20 یا اس سے جدید ورژن والا مینٹینڈ کنٹرولر استعمال کریں۔

Molecule کنورجنس، بغیر کسی تبدیلی کے دوسرے رن، لوڈ کردہ رول سیٹ، SSH پورٹ 22 کی اجازت دینے والے رول، کنفیگریشن کی اجازتوں (permissions)، اور مصنوعی طور پر پیدا کردہ اپلائی ناکامی کے بعد رول بیک کی جانچ کرتا ہے۔ Enterprise Linux میں `/etc/sysconfig/nftables.conf` استعمال ہوتا ہے؛ Debian اور Ubuntu میں `/etc/nftables.conf` استعمال ہوتا ہے۔ `nftables_conf_path` بدستور کنفیگر کیا جا سکتا ہے۔ اس اوور رائیڈ کو استعمال کرنے سے پہلے مقامی nftables سروس کو کسی بھی کسٹم پاتھ کو لوڈ کرنے کے لیے کنفیگر کریں: یہ رول سروس کو ری اسٹارٹ کرتا ہے، جو بصورت دیگر اپنی اصل (stock) فائل لوڈ کرتی ہے اور مطلوبہ رول سیٹ کو تبدیل کر سکتی ہے۔

Enterprise Linux پر، ایک مختلف پرانی `/etc/nftables.conf` فائل پروموشن سے پہلے رول کو روک دیتی ہے۔ دونوں فائلوں کا بیک اپ لیں، پھر توثیق شدہ پچھلی کنفیگریشن کو مقامی سروس پاتھ پر منتقل کریں۔ متبادل کے طور پر، واضح طور پر پرانا پاتھ برقرار رکھیں اور سروس کو اسی کے مطابق کنفیگر کریں۔ `/etc/nftables.conf` پر کوئی پرانی کاپی نہ چھوڑیں: وہاں مختلف فائل کی موجودگی بعد کے رنز میں گارڈ کو فعال کر دیتی ہے۔ اپ گریڈ کے دوران منتخب کردہ پاتھ پر پچھلے رولز رول بیک سورس کے طور پر دستیاب رہتے ہیں۔

Rocky Linux، Enterprise Linux کے پیکیج اور سروس پاتھ کی جانچ کرتا ہے۔ یہ کنٹینر ٹیسٹ RHEL سبسکرپشنز یا پروڈکشن کرنل کی تصدیق نہیں کرتے۔ توسیعی سپورٹ کے بغیر Ubuntu 20.04، Debian 11 اور EL8 کا پرانا Python موجودہ ٹیسٹ میٹرکس سے باہر ہیں۔
