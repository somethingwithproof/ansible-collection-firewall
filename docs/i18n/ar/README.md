<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · **العربية** · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · **العربية** · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

مجموعة Ansible لوصف سياسة جدار الحماية للمضيف وتطبيق تهيئة nftables. تفصل مجموعات العناوين السياسةَ عن مجموعة القواعد المُنشأة.

## التنفيذ والحدود

- يتولى الدور [firewall](../../../roles/firewall/tasks/main.yml) الإعدادات الافتراضية العامة والتحقق من الواجهة الخلفية؛ ويحافظ الدور [policy](../../../roles/policy/tasks/main.yml) على نقطة الدخول القديمة من خلال استيراده.
- يثبّت الدور [nftables](../../../roles/nftables/tasks/main.yml) حزمة nftables، ويجهّز التهيئة مرحلياً، ويتحقق من صحتها باستخدام `nft -c -f`، ثم يطبّقها باستخدام `nft -f`.
- **`nftables_backup: true`** — تحاول كتلة التطبيق استعادة التهيئة السابقة عند حدوث فشل في حال وجودها. ويظل توفر النسخ الاحتياطية والوصول للاسترداد من المتطلبات التشغيلية.
- يحدّد [القالب](../../../roles/nftables/templates/nftables.conf.j2) و[الإعدادات الافتراضية](../../../roles/firewall/defaults/main.yml) حقول السياسة المدعومة.

تتضمن المجموعة الأدوار `firewall` و`policy` و`nftables`؛ ولا ينفّذ واجهةً خلفية لجدار الحماية سوى `nftables`. يقبل التحقق من السياسة أسماء الواجهات الخلفية لكل من iptables وfirewalld، لكن لا توجد أدوار خلفية مقابلة مرفقة هنا. وجود قاعدة SSH في القالب لا يضمن عدم فقدان الوصول. ولا يزال وضع التحقق فقط يثبّت الحزم وينفّذ مهام التهيئة والخدمات؛ أي إنه ليس تشغيلاً تجريبياً خالياً من الآثار الجانبية.

## البدء السريع

ابنِ المجموعة وثبّتها من هذه النسخة المستخرجة:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

يتبع اسم الملف رقم الإصدار المذكور في [galaxy.yml](../../../galaxy.yml). استخدم اسم الملف الناتج عن البناء إذا تغيّر ذلك الإصدار.

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

راجع السياسة المُنشأة على مضيف اختبار قبل تطبيقها على المضيفات البعيدة. واحتفظ بمسار استرداد مستقل عند تعديل سياسة جدار الحماية.

## التطوير

يصف [سير عمل التكامل المستمر (CI)](../../../.github/workflows/ci.yml) و[سيناريوهات Molecule](../../../molecule) عمليات التحقق المُهيّأة. ولا يثبت وجودهما نجاح النشر على كل توزيعة.

## الامتثال والترخيص

أشار ملف README السابق إلى وثيقة لمطابقة الامتثال غير موجودة في هذه النسخة المستخرجة. لا يُدّعى هنا أي اعتماد أو امتثال لأي إطار عمل. تستخدم المجموعة [رخصة MIT](../../../LICENSE)؛ ويسجّل ملف [galaxy.yml](../../../galaxy.yml) البيانات الوصفية للحزمة.

## دعم أنظمة التشغيل

تُختبر الواجهة الخلفية nftables مع Ansible core 2.21 على Ubuntu 22.04 و24.04 و26.04 LTS، وDebian 12 و13، وRocky Linux 9 و10. استخدم عقدة تحكم مدعومة تعمل بإصدار core 2.20 أو أحدث لدعم Python 3.14 في Ubuntu 26.04.

يتحقق Molecule من التقارب (convergence)، ومن تشغيل ثانٍ دون تغييرات، ومن مجموعة القواعد المحمّلة، وقاعدة السماح لمنفذ SSH رقم 22، وأذونات ملف التهيئة، والتراجع (rollback) بعد حقن فشل في التطبيق. يستخدم Enterprise Linux المسار `/etc/sysconfig/nftables.conf`؛ بينما يستخدم Debian وUbuntu المسار `/etc/nftables.conf`. ويظل المتغير `nftables_conf_path` قابلاً للتهيئة. هيّئ خدمة nftables الأصلية لتحميل أي مسار مخصّص قبل استخدام هذا التجاوز: إذ يعيد الدور تشغيل الخدمة، وإلا فستحمّل ملفها الافتراضي وقد تستبدل مجموعة القواعد المقصودة.

في Enterprise Linux، يؤدي وجود ملف `/etc/nftables.conf` قديم ومختلف إلى إيقاف الدور قبل الترقية والتفعيل. انسخ كلا الملفين احتياطياً، ثم انقل التهيئة السابقة المتحقَّق من صحتها إلى مسار الخدمة الأصلي. أو بدلاً من ذلك، احتفظ صراحةً بالمسار القديم وهيّئ الخدمة وفقاً لذلك. لا تترك نسخة قديمة في `/etc/nftables.conf`: فوجود ملف مختلف هناك يُفعّل آلية الحماية في التشغيلات اللاحقة. وتبقى القواعد السابقة متاحة في المسار المحدّد كمصدر للتراجع أثناء الترقية.

تختبر Rocky Linux مسار الحزم والخدمات الخاص بـ Enterprise Linux. ولا تُعدّ اختبارات الحاويات هذه شهادةً لاشتراكات RHEL أو لنواة إنتاجية. ويقع كل من Ubuntu 20.04 دون الدعم الممتد، وDebian 11، وإصدار Python الأقدم في EL8 خارج مصفوفة الاختبار الحالية.
