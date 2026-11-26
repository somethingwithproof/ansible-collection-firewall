<!-- translations:start -->
**Languages:** [English](../../../README.md) · **বাংলা** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** **বাংলা** · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

হোস্ট ফায়ারওয়াল নীতি সংজ্ঞায়িত করা এবং nftables কনফিগারেশন প্রয়োগ করার জন্য একটি Ansible কালেকশন। অ্যাড্রেস গ্রুপগুলো নীতিকে তৈরি হওয়া রুলসেট থেকে আলাদা রাখে।

## বাস্তবায়ন ও সীমাবদ্ধতা

- [firewall](../../../roles/firewall/tasks/main.yml) পাবলিক ডিফল্ট এবং ব্যাকএন্ড যাচাইকরণ পরিচালনা করে; [policy](../../../roles/policy/tasks/main.yml) এটি ইম্পোর্ট করে লিগ্যাসি এন্ট্রি পয়েন্ট বজায় রাখে।
- [nftables](../../../roles/nftables/tasks/main.yml) nftables ইনস্টল করে, কনফিগারেশন স্টেজ করে, `nft -c -f` দিয়ে তা যাচাই করে এবং `nft -f` দিয়ে প্রয়োগ করে।
- কোনো ব্যর্থতার পর পূর্ববর্তী কনফিগারেশন বিদ্যমান থাকলে প্রয়োগ (apply) ব্লকটি তা পুনরুদ্ধার করার চেষ্টা করে। ব্যাকআপের প্রাপ্যতা এবং পুনরুদ্ধার অ্যাক্সেস অপারেশনাল আবশ্যকতা হিসেবে রয়ে যায়।
- [টেমপ্লেট](../../../roles/nftables/templates/nftables.conf.j2) এবং [ডিফল্ট মানসমূহ](../../../roles/policy/defaults/main.yml) সমর্থিত নীতির ফিল্ডগুলো নির্ধারণ করে।

এই কালেকশনটি `firewall`, `policy`, এবং `nftables` প্যাকেজ করে; শুধুমাত্র `nftables` একটি ফায়ারওয়াল ব্যাকএন্ড বাস্তবায়ন করে। নীতি যাচাইকরণে iptables এবং firewalld-এর ব্যাকএন্ড নাম গ্রহণ করা হলেও, এখানে সংশ্লিষ্ট কোনো ব্যাকএন্ড রোল সরবরাহ করা হয়নি। টেমপ্লেটে একটি SSH রুল থাকা মানেই লকআউট না হওয়ার নিশ্চয়তা নয়। কেবল যাচাইকরণ (validate-only) মোডেও প্যাকেজ ইনস্টল হয় এবং কনফিগারেশন/সার্ভিস টাস্ক সম্পন্ন হয়; এটি কোনো পার্শ্বপ্রতিক্রিয়ামুক্ত ড্রাই রান নয়।

## দ্রুত শুরু

এই চেকআউট থেকে কালেকশনটি বিল্ড এবং ইনস্টল করুন:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

ফাইলের নাম [galaxy.yml](../../../galaxy.yml)-এ থাকা সংস্করণ অনুসরণ করে। সেই সংস্করণ পরিবর্তিত হলে বিল্ড থেকে উৎপন্ন ফাইলের নাম ব্যবহার করুন।

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

রিমোট হোস্টে প্রয়োগ করার আগে একটি টেস্ট হোস্টে তৈরি হওয়া নীতি পর্যালোচনা করুন। ফায়ারওয়াল নীতি পরিবর্তন করার সময় একটি স্বতন্ত্র পুনরুদ্ধার পথ বজায় রাখুন।

## ডেভেলপমেন্ট

[CI ওয়ার্কফ্লো](../../../.github/workflows/ci.yml) এবং [Molecule সিনারিওসমূহ](../../../molecule) কনফিগার করা যাচাইকরণ প্রক্রিয়া বর্ণনা করে। এগুলোর উপস্থিতি প্রতিটি ডিস্ট্রিবিউশনে সফল ডিপ্লয়মেন্টের নিশ্চয়তা দেয় না।

## কমপ্লায়েন্স এবং লাইসেন্স

পূর্ববর্তী README-তে একটি কমপ্লায়েন্স-ম্যাপিং নথির উল্লেখ ছিল, যা এই চেকআউটে নেই। এখানে কোনো সার্টিফিকেশন বা ফ্রেমওয়ার্ক কমপ্লায়েন্স দাবি করা হয়নি। কালেকশনটি [MIT লাইসেন্স](../../../LICENSE) ব্যবহার করে; [galaxy.yml](../../../galaxy.yml) প্যাকেজের মেটাডেটা সংরক্ষণ করে।

## অপারেটিং-সিস্টেম সমর্থন

nftables ব্যাকএন্ডটি Ansible core 2.21-এর সাথে Ubuntu 22.04, 24.04 ও 26.04 LTS, Debian 12 ও 13, এবং Rocky Linux 9 ও 10-এ পরীক্ষিত। Ubuntu 26.04-এর Python 3.14-এর জন্য core 2.20 বা তার নতুন সংস্করণসহ একটি রক্ষণাবেক্ষণকৃত কন্ট্রোলার ব্যবহার করুন।

Molecule কনভার্জেন্স, কোনো পরিবর্তন ছাড়া দ্বিতীয় রান, লোড করা রুলসেট, একটি SSH পোর্ট 22 অনুমতি (allow) রুল, কনফিগারেশন অনুমতি (permissions) এবং ইচ্ছাকৃতভাবে ঘটানো প্রয়োগ ব্যর্থতার পর রোলব্যাক যাচাই করে। Enterprise Linux `/etc/sysconfig/nftables.conf` ব্যবহার করে; Debian এবং Ubuntu `/etc/nftables.conf` ব্যবহার করে। `nftables_conf_path` কনফিগারযোগ্য রয়ে গেছে। এই ওভাররাইড ব্যবহার করার আগে নেটিভ nftables সার্ভিসটিকে যেকোনো কাস্টম পথ লোড করার জন্য কনফিগার করুন: রোলটি সার্ভিসটি রিস্টার্ট করে, যা অন্যথায় তার ডিফল্ট ফাইল লোড করে এবং কাঙ্ক্ষিত রুলসেটটি প্রতিস্থাপন করে দিতে পারে।

Enterprise Linux-এ একটি ভিন্ন লিগ্যাসি `/etc/nftables.conf` ফাইল প্রমোশনের আগে রোলটিকে থামিয়ে দেয়। উভয় ফাইলের ব্যাকআপ নিন, তারপর যাচাইকৃত পূর্ববর্তী কনফিগারেশনটি নেটিভ সার্ভিস পথে স্থানান্তর করুন। বিকল্পভাবে, সুস্পষ্টভাবে লিগ্যাসি পথটি ধরে রাখুন এবং সেই অনুযায়ী সার্ভিসটি কনফিগার করুন। `/etc/nftables.conf`-এ কোনো পুরোনো কপি রেখে দেবেন না: সেখানে একটি ভিন্ন ফাইল থাকলে পরবর্তী রানগুলোতে গার্ডটি সক্রিয় হয়। আপগ্রেডের সময় রোলব্যাক উৎস হিসেবে পূর্ববর্তী রুলগুলো নির্বাচিত পথে উপলব্ধ থাকে।

Rocky Linux Enterprise Linux-এর প্যাকেজ এবং সার্ভিস পথ পরীক্ষা করে। এই কন্টেইনার পরীক্ষাগুলো RHEL সাবস্ক্রিপশন বা প্রোডাকশন কার্নেলের সার্টিফিকেশন প্রদান করে না। বর্ধিত সমর্থনবিহীন Ubuntu 20.04, Debian 11 এবং EL8-এর পুরোনো Python বর্তমান টেস্ট ম্যাট্রিক্সের বাইরে।
