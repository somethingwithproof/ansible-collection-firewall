<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · **हिन्दी**

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · **हिन्दी** · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

होस्ट फ़ायरवॉल नीति का वर्णन करने और nftables कॉन्फ़िगरेशन लागू करने के लिए एक Ansible कलेक्शन। एड्रेस ग्रुप नीति को जनरेट किए गए रूलसेट से अलग रखते हैं।

## कार्यान्वयन और सीमाएँ

- [firewall](../../../roles/firewall/tasks/main.yml) सार्वजनिक डिफ़ॉल्ट और बैकएंड सत्यापन का प्रबंधन करता है; [policy](../../../roles/policy/tasks/main.yml) इसे इम्पोर्ट करके लेगेसी एंट्री पॉइंट को बनाए रखता है।
- [nftables](../../../roles/nftables/tasks/main.yml) nftables को इंस्टॉल करता है, कॉन्फ़िगरेशन को स्टेज करता है, `nft -c -f` से इसकी वैधता जाँचता है, और `nft -f` के साथ इसे लागू करता है।
- विफलता की स्थिति में अप्लाई ब्लॉक पिछला कॉन्फ़िगरेशन मौजूद होने पर उसे पुनर्स्थापित करने का प्रयास करता है। बैकअप की उपलब्धता और रिकवरी एक्सेस अभी भी परिचालन आवश्यकताएँ बनी रहती हैं।
- [टेम्पलेट](../../../roles/nftables/templates/nftables.conf.j2) और [डिफ़ॉल्ट](../../../roles/policy/defaults/main.yml) समर्थित नीति फ़ील्ड को परिभाषित करते हैं।

इस कलेक्शन में `firewall`, `policy`, और `nftables` पैकेज किए गए हैं; केवल `nftables` ही फ़ायरवॉल बैकएंड को लागू करता है। नीति सत्यापन में iptables और firewalld के बैकएंड नाम स्वीकार किए जाते हैं, लेकिन यहाँ उनके संगत बैकएंड रोल शामिल नहीं किए गए हैं। टेम्पलेट में SSH नियम होना लॉकआउट से सुरक्षा की गारंटी नहीं है। केवल-सत्यापन (validate-only) मोड भी पैकेज इंस्टॉल करता है और कॉन्फ़िगरेशन/सेवा कार्य निष्पादित करता है; यह बिना किसी साइड-इफ़ेक्ट वाला ड्राई रन नहीं है।

## त्वरित शुरुआत

इस चेकआउट से कलेक्शन को बिल्ड और इंस्टॉल करें:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

फ़ाइल का नाम [galaxy.yml](../../../galaxy.yml) में दिए गए संस्करण के अनुसार होता है। यदि वह संस्करण बदलता है, तो बिल्ड द्वारा जनरेट किए गए फ़ाइल नाम का उपयोग करें।

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

रिमोट होस्ट पर लागू करने से पहले किसी टेस्ट होस्ट पर जनरेट की गई नीति की समीक्षा करें। फ़ायरवॉल नीति बदलते समय एक स्वतंत्र रिकवरी मार्ग बनाए रखें।

## विकास

[CI वर्कफ़्लो](../../../.github/workflows/ci.yml) और [Molecule परिदृश्य](../../../molecule) कॉन्फ़िगर किए गए सत्यापन का वर्णन करते हैं। इनकी उपस्थिति प्रत्येक डिस्ट्रीब्यूशन पर सफल परिनियोजन को सिद्ध नहीं करती है।

## अनुपालन और लाइसेंस

पहले के README में एक अनुपालन-मैपिंग दस्तावेज़ का संदर्भ दिया गया था जो इस चेकआउट में मौजूद नहीं है। यहाँ किसी भी प्रमाणन या फ़्रेमवर्क अनुपालन का दावा नहीं किया गया है। यह कलेक्शन [MIT लाइसेंस](../../../LICENSE) का उपयोग करता है; [galaxy.yml](../../../galaxy.yml) पैकेज मेटाडेटा को रिकॉर्ड करता है।

## ऑपरेटिंग-सिस्टम समर्थन

nftables बैकएंड का परीक्षण Ubuntu 22.04, 24.04 और 26.04 LTS, Debian 12 और 13, तथा Rocky Linux 9 और 10 पर Ansible core 2.21 के साथ किया गया है। Ubuntu 26.04 के Python 3.14 के लिए core 2.20 या उससे नए संस्करण वाले एक अनुरक्षित कंट्रोलर का उपयोग करें।

Molecule कन्वर्जेंस, बिना किसी बदलाव के दूसरे रन, लोड किए गए रूलसेट, SSH पोर्ट 22 अनुमति नियम, कॉन्फ़िगरेशन अनुमतियों और इंजेक्ट की गई अप्लाई विफलता के बाद रोलबैक की जाँच करता है। Enterprise Linux `/etc/sysconfig/nftables.conf` का उपयोग करता है; Debian और Ubuntu `/etc/nftables.conf` का उपयोग करते हैं। `nftables_conf_path` कॉन्फ़िगर करने योग्य रहता है। किसी भी कस्टम पाथ के ओवरराइड का उपयोग करने से पहले नेटिव nftables सेवा को उस पाथ को लोड करने के लिए कॉन्फ़िगर करें: यह रोल सेवा को पुनः प्रारंभ करता है, जो अन्यथा अपनी डिफ़ॉल्ट फ़ाइल लोड करती है और अपेक्षित रूलसेट को बदल सकती है।

Enterprise Linux पर, एक भिन्न लेगेसी `/etc/nftables.conf` फ़ाइल प्रमोशन से पहले रोल को रोक देती है। दोनों फ़ाइलों का बैकअप लें, फिर सत्यापित पिछले कॉन्फ़िगरेशन को नेटिव सेवा पाथ पर ले जाएँ। वैकल्पिक रूप से, स्पष्ट रूप से लेगेसी पाथ को बनाए रखें और उसके अनुसार सेवा को कॉन्फ़िगर करें। `/etc/nftables.conf` पर कोई पुरानी प्रति न छोड़ें: वहाँ मौजूद भिन्न फ़ाइल बाद के रन में सुरक्षा जाँच को सक्रिय कर देती है। अपग्रेड के दौरान पिछले नियम रोलबैक स्रोत के रूप में चयनित पाथ पर उपलब्ध रहते हैं।

Rocky Linux, Enterprise Linux के पैकेज और सेवा पाथ का परीक्षण करता है। ये कंटेनर परीक्षण RHEL सब्सक्रिप्शन या प्रोडक्शन कर्नेल को प्रमाणित नहीं करते हैं। विस्तारित समर्थन के बिना Ubuntu 20.04, Debian 11 और EL8 का पुराना Python वर्तमान परीक्षण मैट्रिक्स से बाहर हैं।
