<!-- translations:start -->
**Languages:** [English](../../../README.md) · **አማርኛ** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · **አማርኛ** · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

የአስተናጋጅ ፋየርዎል ፖሊሲን ለመግለጽ እና የnftables ውቅርን ተግባራዊ ለማድረግ የሚያገለግል የAnsible ስብስብ። የአድራሻ ቡድኖች ፖሊሲውን ከሚመነጨው የሕጎች ስብስብ ተለይቶ እንዲቆይ ያደርጋሉ።

## አተገባበር እና ወሰኖች

- [firewall](../../../roles/firewall/tasks/main.yml) ይፋዊ ነባሪዎችን እና የጀርባ አገልግሎት ማረጋገጫን ይይዛል፤ [policy](../../../roles/policy/tasks/main.yml) ደግሞ እሱን በማስገባት የቀድሞውን የመግቢያ ነጥብ ይጠብቃል።
- [nftables](../../../roles/nftables/tasks/main.yml) nftablesን ይጭናል፣ ውቅሩን ያዘጋጃል፣ በ`nft -c -f` ያረጋግጠዋል፣ እንዲሁም በ`nft -f` ተግባራዊ ያደርገዋል።
- የመተግበር ክፍሉ ብልሽት ሲያጋጥም ቀደም ሲል የነበረ ውቅር ካለ ወደ ቀድሞው ለመመለስ ይሞክራል። የመጠባበቂያ ቅጂ መኖር እና የመልሶ ማግኛ መዳረሻ የአሠራር መስፈርቶች ሆነው ይቀጥላሉ።
- [አብነት](../../../roles/nftables/templates/nftables.conf.j2) እና [ነባሪዎች](../../../roles/policy/defaults/main.yml) የሚደገፉ የፖሊሲ መስኮችን ይገልጻሉ።

ስብስቡ `firewall`፣ `policy` እና `nftables`ን ያካትታል፤ ነገር ግን የፋየርዎል ጀርባ አገልግሎትን ተግባራዊ የሚያደርገው `nftables` ብቻ ነው። የiptables እና የfirewalld የጀርባ አገልግሎት ስሞች በፖሊሲ ማረጋገጫ ተቀባይነት አላቸው፣ ነገር ግን ተጓዳኝ የጀርባ አገልግሎት ሚናዎች እዚህ አልተካተቱም። በአብነቱ ውስጥ ያለ የSSH ሕግ ከመዳረሻ መዘጋት ዋስትና አይሰጥም። የማረጋገጫ ብቻ ሁነታ አሁንም ጥቅሎችን ይጭናል እንዲሁም የውቅር እና የአገልግሎት ተግባራትን ያከናውናል፤ ምንም የጎንዮሽ ውጤት የሌለው ደረቅ ሙከራ አይደለም።

## ፈጣን ጅማሬ

ስብስቡን ከዚህ ቅጂ ይገንቡ እና ይጫኑ፦

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

የፋይሉ ስም በ[galaxy.yml](../../../galaxy.yml) ውስጥ ያለውን ሥሪት ይከተላል። ያ ሥሪት ከተለወጠ በግንባታው የተፈጠረውን የፋይል ስም ይጠቀሙ።

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

የተመነጨውን ፖሊሲ በርቀት አስተናጋጆች ላይ ከመተግበርዎ በፊት በሙከራ አስተናጋጅ ላይ ይገምግሙት። የፋየርዎል ፖሊሲን ሲቀይሩ ራሱን የቻለ የመልሶ ማግኛ መንገድ ይኑርዎት።

## ልማት

[የCI የሥራ ፍሰት](../../../.github/workflows/ci.yml) እና [የMolecule ሁኔታዎች](../../../molecule) የተዋቀረውን ማረጋገጫ ይገልጻሉ። የእነሱ መኖር በእያንዳንዱ ሥርዓተ ክወና ሥርጭት ላይ የተሳካ ማሰማራትን አያረጋግጥም።

## ተገዢነት እና ፈቃድ

ቀደም ሲል የነበረው README በዚህ ቅጂ ውስጥ የማይገኝ የተገዢነት ማጣመሪያ ሰነድ ጠቅሶ ነበር። እዚህ ምንም ዓይነት የምስክር ወረቀት ወይም የማዕቀፍ ተገዢነት አይገለጽም። ስብስቡ [የMIT ፈቃድ](../../../LICENSE)ን ይጠቀማል፤ [galaxy.yml](../../../galaxy.yml) የጥቅሉን ሜታዳታ ይመዘግባል።

## የሥርዓተ ክወና ድጋፍ

የnftables ጀርባ አገልግሎት በUbuntu 22.04፣ 24.04 እና 26.04 LTS፣ Debian 12 እና 13፣ እንዲሁም Rocky Linux 9 እና 10 ላይ በAnsible core 2.21 ተፈትሿል። ለUbuntu 26.04 Python 3.14፣ core 2.20 ወይም ከዚያ በላይ ያለው የሚደገፍ ተቆጣጣሪ ይጠቀሙ።

Molecule መጣጣምን፣ ምንም ለውጥ የሌለበትን ሁለተኛ ሩጫ፣ የተጫነውን የሕጎች ስብስብ፣ የSSH ወደብ 22 መፍቀጃ ሕግን፣ የውቅር ፈቃዶችን እና ሆን ተብሎ ከተፈጠረ የመተግበር ብልሽት በኋላ ወደ ቀድሞው መመለስን ይፈትሻል። Enterprise Linux `/etc/sysconfig/nftables.conf`ን ይጠቀማል፤ Debian እና Ubuntu `/etc/nftables.conf`ን ይጠቀማሉ። `nftables_conf_path` ሊዋቀር የሚችል ሆኖ ይቀጥላል። ያንን ተለዋጭ ከመጠቀምዎ በፊት ማንኛውንም ብጁ ዱካ እንዲጭን የነባሩን nftables አገልግሎት ያዋቅሩ፦ ሚናው አገልግሎቱን እንደገና ያስጀምረዋል፣ ይህ ካልሆነ ነባሪ ፋይሉን ይጭናል እና የታሰበውን የሕጎች ስብስብ ሊተካ ይችላል።

በEnterprise Linux ላይ፣ የተለየ የቀድሞ `/etc/nftables.conf` ፋይል ሚናው ከመተግበሩ በፊት እንዲቆም ያደርገዋል። የሁለቱንም ፋይሎች መጠባበቂያ ቅጂ ይያዙ፣ ከዚያም የተረጋገጠውን የቀድሞ ውቅር ወደ ነባሩ የአገልግሎት ዱካ ያዛውሩ። በአማራጭ፣ የቀድሞውን ዱካ በግልጽ ይጠብቁ እና አገልግሎቱን በዚያው መሠረት ያዋቅሩ። በ`/etc/nftables.conf` ላይ ያረጀ ቅጂ አይተዉ፦ እዚያ ያለ የተለየ ፋይል በቀጣይ ሩጫዎች ላይ ጥበቃውን ያስነሳል። የቀድሞዎቹ ሕጎች በማሻሻያ ጊዜ እንደ መመለሻ ምንጭ በተመረጠው ዱካ ላይ ይቆያሉ።

Rocky Linux የEnterprise Linux ጥቅልን እና የአገልግሎት ዱካን ይፈትሻል። እነዚህ የኮንቴይነር ሙከራዎች የRHEL ምዝገባዎችን ወይም የምርት ከርነልን አያረጋግጡም። ያለ ተጨማሪ ድጋፍ Ubuntu 20.04፣ Debian 11 እና የEL8 የቀድሞ Python አሁን ካለው የሙከራ ማትሪክስ ውጪ ናቸው።
