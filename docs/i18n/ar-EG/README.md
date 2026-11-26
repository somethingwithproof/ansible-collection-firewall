<!-- translations:start -->
**Languages:** [English](../../../README.md) · **العربية (مصر)** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · **العربية (مصر)** · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

مجموعة (collection) لـ Ansible علشان توصف سياسة الـ firewall للسيرفر وتطبّق إعدادات nftables. مجموعات العناوين (Address groups) بتفصل السياسة عن مجموعة القواعد (ruleset) اللي بتتولد.

## التنفيذ والحدود

- دور [firewall](../../../roles/firewall/tasks/main.yml) مسؤول عن الإعدادات الافتراضية العامة والتحقق من الـ backend؛ ودور [policy](../../../roles/policy/tasks/main.yml) بيحافظ على نقطة الدخول القديمة عن طريق استيراده.
- دور [nftables](../../../roles/nftables/tasks/main.yml) بيثبّت nftables، وبيجهّز ملف الإعدادات، وبيتحقق منه بـ `nft -c -f`، وبيطبّقه بـ `nft -f`.
- **`nftables_backup: true`** — بلوك التطبيق بيحاول يرجّع الإعدادات السابقة بعد أي فشل لو كانت موجودة قبل كده. توفر النسخ الاحتياطية وطريقة الاسترداد لسه متطلبات تشغيلية أساسية.
- [القالب](../../../roles/nftables/templates/nftables.conf.j2) و[الإعدادات الافتراضية](../../../roles/firewall/defaults/main.yml) بيحددوا حقول السياسة المدعومة.

المجموعة فيها أدوار `firewall` و`policy` و`nftables`؛ لكن `nftables` بس هو اللي بينفّذ backend للـ firewall. أسماء الـ backend بتاعة iptables وfirewalld مقبولة في التحقق من السياسة، بس مفيش أدوار backend مقابلة ليهم موجودة هنا. وجود قاعدة SSH في القالب مش ضمان ضد قفل الوصول (lockout). وضع التحقق بس (validate-only) لسه بيثبّت حزم وبينفّذ مهام إعدادات وخدمات؛ يعني مش تشغيل تجريبي من غير أي تأثير جانبي.

## البداية السريعة

ابني وثبّت المجموعة من النسخة دي:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

اسم الملف بيتبع الإصدار الموجود في [galaxy.yml](../../../galaxy.yml). استخدم اسم الملف اللي طلع من عملية البناء لو الإصدار ده اتغيّر.

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
        office: ["203.0.113.0/24"]  # استبدلها بشبكة مكتبك (CIDR)
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

راجع السياسة اللي اتولدت على سيرفر تجريبي قبل ما تطبقها على السيرفرات البعيدة. خليك دايمًا محتفظ بمسار استرداد مستقل لما تغيّر سياسة الـ firewall.

## التطوير

[سير عمل الـ CI](../../../.github/workflows/ci.yml) و[سيناريوهات Molecule](../../../molecule) بيوصفوا عمليات التحقق المضبوطة. وجودهم مش بيثبت إن النشر هينجح على كل توزيعة.

## الامتثال والرخصة

الـ README القديم كان بيشير لمستند ربط بالامتثال (compliance-mapping) بس الملف ده مش موجود في النسخة دي. مفيش أي ادعاء بشهادة أو امتثال لأي إطار عمل هنا. المجموعة بتستخدم [رخصة MIT](../../../LICENSE)؛ وملف [galaxy.yml](../../../galaxy.yml) بيسجّل البيانات الوصفية للحزمة.

## دعم أنظمة التشغيل

الـ backend الخاص بـ nftables متجرّب مع Ansible core 2.21 على Ubuntu 22.04 و24.04 و26.04 LTS، وDebian 12 و13، وRocky Linux 9 و10. استخدم controller مدعوم عليه core 2.20 أو أحدث علشان Python 3.14 الموجود في Ubuntu 26.04.

Molecule بيتحقق من التقارب (convergence)، وتشغيل تاني من غير تغييرات، ومجموعة القواعد المحمّلة، وقاعدة السماح لـ SSH على بورت 22، وصلاحيات ملف الإعدادات، والرجوع للإصدار السابق (rollback) بعد فشل تطبيق محقون للتجربة. أنظمة Enterprise Linux بتستخدم `/etc/sysconfig/nftables.conf`؛ بينما Debian وUbuntu بيستخدموا `/etc/nftables.conf`. المتغير `nftables_conf_path` لسه قابل للتخصيص. اضبط خدمة nftables الأساسية علشان تحمّل أي مسار مخصص قبل ما تستخدم التعديل ده: الدور بيعمل إعادة تشغيل للخدمة، واللي لو ما اتضبطتش هتحمّل ملفها الافتراضي وممكن تستبدل القواعد المقصودة.

على Enterprise Linux، لو فيه ملف قديم مختلف في `/etc/nftables.conf`، الدور بيوقف قبل الترقية والتطبيق. خد نسخة احتياطية من الملفين، وبعدين انقل الإعدادات السابقة المتحقَّق منها لمسار الخدمة الأساسي. أو بديلًا عن كده، احتفظ بالمسار القديم بشكل صريح واضبط الخدمة على أساسه. ما تسيبش نسخة قديمة في `/etc/nftables.conf`: وجود ملف مختلف هناك بيفعّل الحماية وبيوقف التشغيل في المرات اللي بعد كده. القواعد السابقة بتفضل متاحة في المسار المختار كمصدر للـ rollback أثناء الترقية.

Rocky Linux بيتستخدم لاختبار حزم ومسار خدمة Enterprise Linux. اختبارات الحاويات (containers) دي مش بتعتمد اشتراكات RHEL أو نواة (kernel) إنتاجية. Ubuntu 20.04 من غير دعم ممتد، وDebian 11، وPython القديم في EL8 بره مصفوفة الاختبار الحالية.
