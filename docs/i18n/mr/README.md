<!-- translations:start -->
**Languages:** [English](../../../README.md) · **मराठी** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · **मराठी** · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

होस्ट फायरवॉल धोरणाचे वर्णन करण्यासाठी आणि nftables कॉन्फिगरेशन लागू करण्यासाठी एक Ansible कलेक्शन. अ‍ॅड्रेस गट (address groups) धोरणाला तयार झालेल्या नियमावलीपासून (ruleset) वेगळे ठेवतात.

## अंमलबजावणी आणि मर्यादा

- [firewall](../../../roles/firewall/tasks/main.yml) सार्वजनिक डीफॉल्ट्स आणि बॅकएंड पडताळणी हाताळते; [policy](../../../roles/policy/tasks/main.yml) ते इम्पोर्ट करून जुना एंट्री पॉइंट कायम ठेवते.
- [nftables](../../../roles/nftables/tasks/main.yml) nftables इन्स्टॉल करते, कॉन्फिगरेशन स्टेज करते, `nft -c -f` वापरून त्याची वैधता तपासते आणि `nft -f` वापरून ते लागू करते.
- **`nftables_backup: true`** — आधीचे कॉन्फिगरेशन अस्तित्वात असल्यास, apply ब्लॉक अयशस्वी झाल्यानंतर ते पुनर्संचयित करण्याचा प्रयत्न करतो. बॅकअपची उपलब्धता आणि रिकव्हरी अ‍ॅक्सेस या ऑपरेशनल आवश्यकता कायम राहतात.
- [टेम्प्लेट](../../../roles/nftables/templates/nftables.conf.j2) आणि [डीफॉल्ट्स](../../../roles/firewall/defaults/main.yml) समर्थित धोरण फील्ड्स परिभाषित करतात.

या कलेक्शनमध्ये `firewall`, `policy`, आणि `nftables` समाविष्ट आहेत; फक्त `nftables` फायरवॉल बॅकएंडची अंमलबजावणी करते. iptables आणि firewalld साठी बॅकएंड नावे धोरण पडताळणीमध्ये स्वीकारली जातात, परंतु त्यासंबंधित कोणतेही बॅकएंड रोल्स येथे दिलेले नाहीत. टेम्प्लेटमधील SSH नियम लॉकआउट होणार नाही याची हमी देत नाही. फक्त-पडताळणी (validate-only) मोडमध्येही पॅकेजेस इन्स्टॉल केली जातात आणि कॉन्फिगरेशन/सर्व्हिस टास्क्स पार पाडली जातात; ते साइड-इफेक्ट-मुक्त ड्राय रन नाही.

## झटपट सुरुवात

या चेकआउटमधून कलेक्शन बिल्ड आणि इन्स्टॉल करा:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

फाइलचे नाव [galaxy.yml](../../../galaxy.yml) मधील आवृत्तीनुसार असते. ती आवृत्ती बदलल्यास, बिल्डद्वारे तयार झालेले फाइलचे नाव वापरा.

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
        office: ["203.0.113.0/24"]  # तुमच्या कार्यालयाच्या नेटवर्कने बदला (CIDR)
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

रिमोट होस्टवर लागू करण्यापूर्वी चाचणी होस्टवर तयार झालेल्या धोरणाचे पुनरावलोकन करा. फायरवॉल धोरण बदलताना एक स्वतंत्र रिकव्हरी मार्ग तयार ठेवा.

## विकास

[CI वर्कफ्लो](../../../.github/workflows/ci.yml) आणि [Molecule सिनॅरिओज](../../../molecule) कॉन्फिगर केलेल्या पडताळणीचे वर्णन करतात. त्यांची उपस्थिती प्रत्येक डिस्ट्रिब्युशनवर यशस्वी डिप्लॉयमेंटची खात्री देत नाही.

## अनुपालन आणि परवाना

आधीच्या README मध्ये अनुपालन-मॅपिंग (compliance-mapping) दस्तऐवजाचा संदर्भ होता, जो या चेकआउटमध्ये उपलब्ध नाही. येथे कोणत्याही प्रमाणपत्राचा किंवा फ्रेमवर्क अनुपालनाचा दावा केला जात नाही. हे कलेक्शन [MIT परवाना](../../../LICENSE) वापरते; [galaxy.yml](../../../galaxy.yml) मध्ये पॅकेज मेटाडेटा नोंदवला आहे.

## ऑपरेटिंग-सिस्टम समर्थन

nftables बॅकएंडची चाचणी Ubuntu 22.04, 24.04 आणि 26.04 LTS, Debian 12 आणि 13, तसेच Rocky Linux 9 आणि 10 वर Ansible core 2.21 सह केली जाते. Ubuntu 26.04 च्या Python 3.14 साठी core 2.20 किंवा त्यानंतरची आवृत्ती असलेला समर्थित कंट्रोलर वापरा.

Molecule कन्व्हर्जन्स, कोणत्याही बदलाशिवाय दुसरे रन, लोड केलेली नियमावली (ruleset), SSH पोर्ट 22 ला परवानगी देणारा नियम, कॉन्फिगरेशन परवानग्या आणि कृत्रिमरीत्या निर्माण केलेल्या apply अयशस्वीतेनंतर रोलबॅक यांची तपासणी करते. Enterprise Linux `/etc/sysconfig/nftables.conf` वापरते; Debian आणि Ubuntu `/etc/nftables.conf` वापरतात. `nftables_conf_path` कॉन्फिगर करण्यायोग्य राहते. हा ओव्हरराइड वापरण्यापूर्वी कोणताही सानुकूल मार्ग लोड करण्यासाठी मूळ nftables सर्व्हिस कॉन्फिगर करा: हा रोल सर्व्हिस रीस्टार्ट करतो, जी अन्यथा तिची डीफॉल्ट फाइल लोड करते आणि अपेक्षित नियमावली बदलू शकते.

Enterprise Linux वर, भिन्न असलेली जुनी `/etc/nftables.conf` फाइल प्रमोशनपूर्वी रोलला थांबवते. दोन्ही फाइल्सचा बॅकअप घ्या, त्यानंतर पडताळलेले आधीचे कॉन्फिगरेशन मूळ सर्व्हिस मार्गावर हलवा. पर्यायाने, जुना मार्ग स्पष्टपणे कायम ठेवा आणि त्यानुसार सर्व्हिस कॉन्फिगर करा. `/etc/nftables.conf` येथे जुनी प्रत ठेवू नका: तेथे भिन्न फाइल असल्यास पुढील रन्समध्ये सुरक्षा प्रतिबंध (guard) कार्यान्वित होतो. अपग्रेड दरम्यान रोलबॅक स्रोत म्हणून निवडलेल्या मार्गावर आधीचे नियम उपलब्ध राहतात.

Rocky Linux हे Enterprise Linux पॅकेज आणि सर्व्हिस मार्गाची चाचणी करते. या कंटेनर चाचण्या RHEL सबस्क्रिप्शन किंवा प्रोडक्शन कर्नलला प्रमाणित करत नाहीत. विस्तारित समर्थनाशिवाय Ubuntu 20.04, Debian 11 आणि EL8 चे जुने Python सध्याच्या चाचणी मॅट्रिक्सच्या बाहेर आहेत.
