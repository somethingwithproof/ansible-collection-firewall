<!-- translations:start -->
**Languages:** [English](../../../README.md) · **עברית** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · **עברית** · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

אוסף Ansible לתיאור מדיניות חומת אש של מארח ולהחלת תצורת nftables. קבוצות כתובות שומרות על הפרדה בין המדיניות לבין סט החוקים שנוצר.

## מימוש וגבולות

- [firewall](../../../roles/firewall/tasks/main.yml) אחראי על ערכי ברירת המחדל הציבוריים ועל אימות המנוע האחורי; [policy](../../../roles/policy/tasks/main.yml) משמר את נקודת הכניסה הישנה על ידי ייבוא שלו.
- [nftables](../../../roles/nftables/tasks/main.yml) מתקין את nftables, מכין את התצורה בשלב ביניים, מאמת אותה באמצעות `nft -c -f` ומחיל אותה באמצעות `nft -f`.
- בלוק ההחלה מנסה לשחזר תצורה קודמת לאחר כשל כאשר קיימת כזו. זמינות גיבויים וגישת שחזור נותרות דרישות תפעוליות.
- [התבנית](../../../roles/nftables/templates/nftables.conf.j2) ו[ברירות המחדל](../../../roles/policy/defaults/main.yml) מגדירים את שדות המדיניות הנתמכים.

האוסף אורז את `firewall`, `policy` ו-`nftables`; רק `nftables` מממש מנוע אחורי של חומת אש. שמות מנועים אחוריים עבור iptables ו-firewalld מתקבלים באימות המדיניות, אך לא מסופקים כאן תפקידים תואמים למנועים אלה. חוק SSH בתבנית אינו מהווה ערובה מפני נעילה מחוץ לשרת. מצב אימות בלבד עדיין מתקין חבילות ומבצע משימות תצורה ושירות; הוא אינו הרצת ניסיון (dry run) ללא תופעות לוואי.

## התחלה מהירה

בנו והתקינו את האוסף מעותק העבודה הנוכחי:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

שם הקובץ נקבע לפי הגרסה המוגדרת ב-[galaxy.yml](../../../galaxy.yml). השתמשו בשם הקובץ שנוצר בבנייה אם גרסה זו משתנה.

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

בדקו את המדיניות שנוצרה על מארח בדיקה לפני החלתה על מארחים מרוחקים. שמרו על נתיב שחזור עצמאי בעת שינוי מדיניות חומת האש.

## פיתוח

[תהליך ה-CI](../../../.github/workflows/ci.yml) ו[תרחישי Molecule](../../../molecule) מתארים את האימות המוגדר. עצם קיומם אינו מוכיח פריסה מוצלחת בכל הפצה.

## תאימות ורישיון

קובץ ה-README הקודם הפנה למסמך מיפוי תאימות שאינו קיים בעותק העבודה הזה. לא נטענת כאן הסמכה או תאימות למסגרת תקנים כלשהי. האוסף מופץ תחת [רישיון MIT](../../../LICENSE); הקובץ [galaxy.yml](../../../galaxy.yml) מתעד את המטא-דאטה של החבילה.

## תמיכה במערכות הפעלה

המנוע האחורי של nftables נבדק עם Ansible core 2.21 על Ubuntu 22.04, 24.04 ו-26.04 LTS, על Debian 12 ו-13, ועל Rocky Linux 9 ו-10. יש להשתמש בבקר מתוחזק עם core 2.20 ומעלה עבור Python 3.14 של Ubuntu 26.04.

Molecule בודק התכנסות, הרצה שנייה ללא שינויים, את סט החוקים שנטען, חוק המאפשר SSH בפורט 22, הרשאות קובצי תצורה וגלילה לאחור (rollback) לאחר כשל החלה יזום. Enterprise Linux משתמש ב-`/etc/sysconfig/nftables.conf`; Debian ו-Ubuntu משתמשות ב-`/etc/nftables.conf`. המשתנה `nftables_conf_path` נשאר ניתן להגדרה. יש להגדיר את שירות nftables המובנה לטעון כל נתיב מותאם אישית לפני שימוש בדריסה זו: התפקיד מפעיל מחדש את השירות, שאחרת יטען את קובץ ברירת המחדל שלו ועלול להחליף את סט החוקים המיועד.

ב-Enterprise Linux, קובץ ישן שונה בנתיב `/etc/nftables.conf` עוצר את התפקיד לפני הקידום. גבו את שני הקבצים, ולאחר מכן העבירו את התצורה הקודמת המאומתת לנתיב השירות המובנה. לחלופין, שמרו במפורש על הנתיב הישן והגדירו את השירות בהתאם. אל תשאירו עותק מיושן ב-`/etc/nftables.conf`: קובץ שונה במיקום זה מפעיל את מנגנון ההגנה בהרצות הבאות. החוקים הקודמים נשארים זמינים בנתיב שנבחר כמקור לגלילה לאחור במהלך שדרוג.

Rocky Linux בוחן את נתיב החבילות והשירות של Enterprise Linux. בדיקות מכולה אלה אינן מאשרות מינויי RHEL או ליבת (kernel) ייצור. Ubuntu 20.04 ללא תמיכה מורחבת, Debian 11 וגרסת Python הישנה של EL8 נמצאים מחוץ למטריצת הבדיקות הנוכחית.
