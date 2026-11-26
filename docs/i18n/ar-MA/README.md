<!-- translations:start -->
**Languages:** [English](../../../README.md) · **العربية (المغرب)** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · **العربية (المغرب)** · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

مجموعة Ansible باش توصف سياسة الجدار الناري (firewall) ديال المضيف وتطبق إعدادات nftables. مجموعات العناوين (Address groups) كيخليو السياسة مفصولة على مجموعة القواعد اللي كيتولدو.

## التنفيذ والحدود

- [firewall](../../../roles/firewall/tasks/main.yml) هو اللي كيتكلف بالإعدادات الافتراضية العمومية والتحقق من الـ backend؛ و[policy](../../../roles/policy/tasks/main.yml) كيحافظ على نقطة الدخول القديمة عبر الاستيراد ديالو.
- [nftables](../../../roles/nftables/tasks/main.yml) كيثبت nftables، كيجهز الإعدادات، كيتحقق منها بـ `nft -c -f`، وكيطبقها بـ `nft -f`.
- البلوك ديال التطبيق كيحاول يرجع الإعدادات السابقة إلى وقع شي مشكل، إلى كانت ديجا كاينة شي نسخة قديمة. توفر النسخ الاحتياطية وطريق الاسترجاع كيبقاو متطلبات تشغيلية ضرورية.
- [القالب](../../../roles/nftables/templates/nftables.conf.j2) و[الإعدادات الافتراضية](../../../roles/policy/defaults/main.yml) كيحددو الحقول ديال السياسة المدعومة.

هاد المجموعة فيها الأدوار `firewall` و`policy` و`nftables`؛ غير `nftables` بوحدو اللي فيه تنفيذ فعلي ديال backend ديال الجدار الناري. السميات ديال backends بحال iptables وfirewalld مقبولين فالتحقق ديال السياسة، ولكن ما كاينينش أدوار backend ديالهم مرفقين هنا. وجود قاعدة SSH فالقالب ماشي ضمانة ضد القفل (lockout) وفقدان الولوج. وضع التحقق بوحدو (Validate-only mode) راه باقي كيثبت الحزم وكيدير مهام الإعدادات والخدمات؛ يعني ماشي تشغيل تجريبي (dry run) بلا تأثيرات جانبية.

## البداية السريعة

بني وثبت المجموعة من هاد النسخة المحلية:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

سمية الملف كتبع الإصدار اللي كاين فـ [galaxy.yml](../../../galaxy.yml). استعمل سمية الملف اللي خرجها البناء إلى تبدل هاد الإصدار.

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

راجع السياسة اللي تولدت على مضيف تجريبي قبل ما تطبقها على المضيفين البعيدين. خلي ديما طريق مستقل للاسترجاع ملي تكون كتبدل سياسة الجدار الناري.

## التطوير

[مسار عمل CI](../../../.github/workflows/ci.yml) و[سيناريوهات Molecule](../../../molecule) كيوصفو التحقق اللي مبرمج. الوجود ديالهم ما كيعنيش ضمان نجاح النشر على كل توزيعة.

## الامتثال والرخصة

ملف README القديم كان كيشير لواحد وثيقة ديال مطابقة الامتثال، ولكن ما كايناش فهاد النسخة. ما كاين حتى ادعاء هنا بشي شهادة أو امتثال لشي إطار عمل. هاد المجموعة كتستعمل [رخصة MIT](../../../LICENSE)؛ و[galaxy.yml](../../../galaxy.yml) كيسجل البيانات الوصفية ديال الحزمة.

## دعم أنظمة التشغيل

الـ backend ديال nftables مجرب مع Ansible core 2.21 على Ubuntu 22.04 و24.04 و26.04 LTS، وDebian 12 و13، وRocky Linux 9 و10. خاصك تستعمل متحكم (controller) مدعوم بـ core 2.20 أو أحدث على قبل Python 3.14 ديال Ubuntu 26.04.

Molecule كيتحقق من التقارب (convergence)، وتشغيل ثاني بلا تغييرات، ومجموعة القواعد المحملة، وقاعدة السماح لمنفذ SSH 22، وصلاحيات الإعدادات، والرجوع للخلف (rollback) من بعد حقن خطأ متعمد فالتطبيق. Enterprise Linux كيستعمل `/etc/sysconfig/nftables.conf`؛ بينما Debian وUbuntu كيستعملو `/etc/nftables.conf`. المتغير `nftables_conf_path` كيبقى قابل للتعديل. خاصك تضبط خدمة nftables الأصلية باش تحمل أي مسار مخصص قبل ما تستعمل هاد التجاوز: الدور كيعاود يشغل الخدمة، واللي إلا ما ضبطتيهاش غادي تحمل ملفها الافتراضي وتقدر تبدل مجموعة القواعد اللي بغيتي.

على Enterprise Linux، وجود ملف قديم مختلف فـ `/etc/nftables.conf` كيوقف الدور قبل الترقية والتطبيق. دير نسخة احتياطية من الملفين بجوج، ومن بعد نقل الإعدادات السابقة المتحقق منها للمسار الأصلي ديال الخدمة. ولا، تقدر تختار بصراحة تخلي المسار القديم وتضبط الخدمة على حسابو. ما تخليش شي نسخة قديمة فـ `/etc/nftables.conf`: وجود ملف مختلف تما غادي يطلق هاد الحماية فالتشغيلات الجاية. القواعد السابقة كتبقى متوفرة فالمسار المختار كمصدر للرجوع للخلف أثناء التحديث.

Rocky Linux كيختبر المسار ديال الحزم والخدمات ديال Enterprise Linux. هاد اختبارات الحاويات ما كتصادقش على اشتراكات RHEL ولا على نواة (kernel) ديال الإنتاج. Ubuntu 20.04 بلا دعم ممتد، وDebian 11، وPython القديم ديال EL8 خارجين على مصفوفة الاختبار الحالية.
