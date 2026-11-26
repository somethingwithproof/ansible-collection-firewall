<!-- translations:start -->
**Languages:** [English](../../../README.md) · **ไทย** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · **ไทย** · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

คอลเลกชัน Ansible สำหรับกำหนดนโยบายไฟร์วอลล์ของโฮสต์และนำการกำหนดค่า nftables ไปใช้งาน กลุ่มที่อยู่ (address groups) ช่วยแยกนโยบายออกจากชุดกฎ (ruleset) ที่ถูกสร้างขึ้น

## การนำไปใช้งานและขอบเขต

- [firewall](../../../roles/firewall/tasks/main.yml) ดูแลค่าเริ่มต้นสาธารณะและการตรวจสอบความถูกต้องของแบ็กเอนด์ ส่วน [policy](../../../roles/policy/tasks/main.yml) ยังคงจุดเข้าใช้งานแบบเดิมไว้โดยการนำเข้า role ดังกล่าว
- [nftables](../../../roles/nftables/tasks/main.yml) ติดตั้ง nftables จัดเตรียมการกำหนดค่า ตรวจสอบความถูกต้องด้วย `nft -c -f` และนำไปใช้งานด้วย `nft -f`
- บล็อกการนำการกำหนดค่าไปใช้จะพยายามกู้คืนการกำหนดค่าก่อนหน้าเมื่อเกิดข้อผิดพลาดหากมีไฟล์เดิมอยู่ ความพร้อมใช้งานของข้อมูลสำรองและช่องทางเข้าถึงเพื่อการกู้คืนยังคงเป็นข้อกำหนดด้านการปฏิบัติงาน
- [เทมเพลต](../../../roles/nftables/templates/nftables.conf.j2) และ[ค่าเริ่มต้น](../../../roles/policy/defaults/main.yml) เป็นตัวกำหนดฟิลด์นโยบายที่รองรับ

คอลเลกชันนี้รวม role `firewall`, `policy` และ `nftables` ไว้ด้วยกัน โดยจะมีเพียง `nftables` เท่านั้นที่พัฒนาแบ็กเอนด์ของไฟร์วอลล์ไว้จริง การตรวจสอบนโยบายยอมรับชื่อแบ็กเอนด์สำหรับ iptables และ firewalld แต่ไม่มี role แบ็กเอนด์ที่สอดคล้องกันรวมอยู่ในคอลเลกชันนี้ กฎ SSH ในเทมเพลตไม่ได้รับประกันว่าจะไม่เกิดการถูกล็อกเอาต์ โหมดตรวจสอบอย่างเดียว (validate-only mode) ยังคงติดตั้งแพ็กเกจและดำเนินงานด้านการกำหนดค่า/เซอร์วิสอยู่ จึงไม่ใช่การทดสอบรัน (dry run) ที่ปราศจากผลข้างเคียง

## เริ่มต้นใช้งานอย่างรวดเร็ว

บิลด์และติดตั้งคอลเลกชันจากโค้ดที่ checkout มา:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

ชื่อไฟล์จะอ้างอิงตามเวอร์ชันใน [galaxy.yml](../../../galaxy.yml) ให้ใช้ชื่อไฟล์ที่ได้จากการบิลด์หากเวอร์ชันดังกล่าวมีการเปลี่ยนแปลง

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

ตรวจสอบนโยบายที่สร้างขึ้นบนโฮสต์ทดสอบก่อนนำไปใช้กับโฮสต์ระยะไกล และควรเตรียมช่องทางการกู้คืนที่เป็นอิสระไว้เสมอเมื่อเปลี่ยนแปลงนโยบายไฟร์วอลล์

## การพัฒนา

[เวิร์กโฟลว์ CI](../../../.github/workflows/ci.yml) และ[สถานการณ์ทดสอบของ Molecule](../../../molecule) อธิบายขั้นตอนการตรวจสอบที่ได้กำหนดค่าไว้ การมีอยู่ของสิ่งเหล่านี้ไม่ได้เป็นเครื่องรับประกันว่าการดีพลอยจะสำเร็จในทุกลินุกซ์ดิสทริบิวชัน

## การปฏิบัติตามข้อกำหนดและสัญญาอนุญาต

README ฉบับก่อนหน้าได้อ้างถึงเอกสารการจับคู่ตามข้อกำหนด (compliance-mapping) ซึ่งไม่มีอยู่ในโค้ดชุดนี้ จึงไม่มีการรับรองมาตรฐานหรือการปฏิบัติตามเฟรมเวิร์กใด ๆ ในที่นี้ คอลเลกชันนี้ใช้[สัญญาอนุญาต MIT](../../../LICENSE) และ [galaxy.yml](../../../galaxy.yml) บันทึกข้อมูลเมตาของแพ็กเกจไว้

## การรองรับระบบปฏิบัติการ

แบ็กเอนด์ nftables ผ่านการทดสอบด้วย Ansible core 2.21 บน Ubuntu 22.04, 24.04 และ 26.04 LTS, Debian 12 และ 13 รวมถึง Rocky Linux 9 และ 10 ควรใช้คอนโทรลเลอร์ที่ยังได้รับการดูแลรักษาพร้อม core 2.20 ขึ้นไปสำหรับ Python 3.14 บน Ubuntu 26.04

Molecule ตรวจสอบการทำงานจนได้สถานะที่ต้องการ (convergence) การรันซ้ำรอบที่สองโดยไม่มีการเปลี่ยนแปลง ชุดกฎที่โหลดใช้งาน กฎอนุญาต SSH พอร์ต 22 สิทธิ์ของไฟล์การกำหนดค่า และการย้อนกลับ (rollback) หลังจำลองความล้มเหลวในการนำการกำหนดค่าไปใช้ Enterprise Linux ใช้ `/etc/sysconfig/nftables.conf` ส่วน Debian และ Ubuntu ใช้ `/etc/nftables.conf` โดยยังคงสามารถกำหนดค่า `nftables_conf_path` ได้ ให้กำหนดค่าเซอร์วิส nftables ของระบบให้โหลดพาธแบบกำหนดเองก่อนใช้งานการเขียนทับค่านี้ เนื่องจาก role จะรีสตาร์ตเซอร์วิส ซึ่งหากไม่ได้ตั้งค่าไว้ เซอร์วิสจะโหลดไฟล์มาตรฐานเดิมและอาจไปแทนที่ชุดกฎที่ตั้งใจไว้

บน Enterprise Linux หากพบไฟล์แบบเดิม `/etc/nftables.conf` ที่มีเนื้อหาแตกต่างกัน role จะหยุดทำงานก่อนการนำการกำหนดค่าใหม่ขึ้นใช้งาน ให้สำรองไฟล์ทั้งสองฉบับไว้ก่อน จากนั้นย้ายการกำหนดค่าก่อนหน้าที่ผ่านการตรวจสอบแล้วไปยังพาธของเซอร์วิสมาตรฐานของระบบ หรืออาจเลือกคงพาธแบบเดิมไว้อย่างชัดเจนแล้วตั้งค่าเซอร์วิสให้สอดคล้องกัน อย่าทิ้งสำเนาที่ล้าสมัยไว้ที่ `/etc/nftables.conf` เพราะไฟล์ที่แตกต่างกัน ณ ตำแหน่งนั้นจะกระตุ้นกลไกป้องกันในการรันครั้งถัดไป กฎเดิมจะยังคงอยู่ที่พาธที่เลือกไว้เพื่อใช้เป็นแหล่งสำหรับการย้อนกลับระหว่างการอัปเกรด

Rocky Linux ใช้ทดสอบเส้นทางของแพ็กเกจและเซอร์วิสในสาย Enterprise Linux การทดสอบด้วยคอนเทนเนอร์เหล่านี้ไม่ได้เป็นการรับรองการสมัครสมาชิก RHEL หรือเคอร์เนลสำหรับสภาพแวดล้อมการผลิต Ubuntu 20.04 ที่ไม่มีการรองรับแบบขยายเวลา, Debian 11 และ Python เวอร์ชันเก่าของ EL8 อยู่นอกเมทริกซ์การทดสอบในปัจจุบัน
