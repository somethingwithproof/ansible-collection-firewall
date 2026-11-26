<!-- translations:start -->
**Languages:** [English](../../../README.md) · **తెలుగు** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · **తెలుగు** · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

హోస్ట్ ఫైర్‌వాల్ పాలసీని నిర్వచించడానికి మరియు nftables కాన్ఫిగరేషన్‌ను వర్తింపజేయడానికి ఉపయోగపడే Ansible కలెక్షన్. అడ్రస్ గ్రూపులు పాలసీని జనరేట్ చేయబడిన రూల్‌సెట్ నుండి వేరుగా ఉంచుతాయి.

## అమలు మరియు పరిమితులు

- [firewall](../../../roles/firewall/tasks/main.yml) పబ్లిక్ డిఫాల్ట్‌లు మరియు బ్యాకెండ్ ధృవీకరణను నిర్వహిస్తుంది; [policy](../../../roles/policy/tasks/main.yml) దానిని ఇంపోర్ట్ చేయడం ద్వారా పాత ఎంట్రీ పాయింట్‌ను కొనసాగిస్తుంది.
- [nftables](../../../roles/nftables/tasks/main.yml) nftablesను ఇన్‌స్టాల్ చేస్తుంది, కాన్ఫిగరేషన్‌ను స్టేజ్ చేస్తుంది, `nft -c -f`తో దానిని ధృవీకరిస్తుంది మరియు `nft -f`తో వర్తింపజేస్తుంది.
- **`nftables_backup: true`** — వర్తింపజేసే బ్లాక్ విఫలమైనప్పుడు, మునుపటి కాన్ఫిగరేషన్ అందుబాటులో ఉంటే దానిని పునరుద్ధరించడానికి ప్రయత్నిస్తుంది. బ్యాకప్ లభ్యత మరియు రికవరీ యాక్సెస్ ఆపరేషనల్ అవసరాలుగా కొనసాగుతాయి.
- [టెంప్లేట్](../../../roles/nftables/templates/nftables.conf.j2) మరియు [డిఫాల్ట్‌లు](../../../roles/firewall/defaults/main.yml) మద్దతు ఉన్న పాలసీ ఫీల్డ్‌లను నిర్వచిస్తాయి.

ఈ కలెక్షన్‌లో `firewall`, `policy`, మరియు `nftables` ప్యాకేజ్ చేయబడ్డాయి; వీటిలో కేవలం `nftables` మాత్రమే ఫైర్‌వాల్ బ్యాకెండ్‌ను అమలు చేస్తుంది. iptables మరియు firewalld కోసం బ్యాకెండ్ పేర్లను పాలసీ ధృవీకరణ అంగీకరిస్తుంది, కానీ వాటికి సంబంధించిన బ్యాకెండ్ రోల్స్ ఇక్కడ అందించబడలేదు. టెంప్లేట్‌లో SSH నియమం ఉండటం వల్ల లాకౌట్ జరగదని హామీ లేదు. వాలిడేట్-ఓన్లీ మోడ్ కూడా ప్యాకేజీలను ఇన్‌స్టాల్ చేస్తుంది మరియు కాన్ఫిగరేషన్/సర్వీస్ టాస్క్‌లను నిర్వహిస్తుంది; ఇది ఎటువంటి మార్పులు చేయని డ్రై రన్ కాదు.

## త్వరిత ప్రారంభం

ఈ చెక్‌అవుట్ నుండి కలెక్షన్‌ను బిల్డ్ చేసి ఇన్‌స్టాల్ చేయండి:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

ఫైల్ పేరు [galaxy.yml](../../../galaxy.yml)లోని వెర్షన్‌ను అనుసరిస్తుంది. ఆ వెర్షన్ మారితే, బిల్డ్ ద్వారా రూపొందించబడిన ఫైల్ పేరును ఉపయోగించండి.

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
        office: ["203.0.113.0/24"]  # మీ కార్యాలయ నెట్‌వర్క్‌తో భర్తీ చేయండి (CIDR)
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

రిమోట్ హోస్ట్‌లకు వర్తింపజేసే ముందు, టెస్ట్ హోస్ట్‌లో జనరేట్ చేయబడిన పాలసీని సమీక్షించండి. ఫైర్‌వాల్ పాలసీని మార్చేటప్పుడు స్వతంత్ర రికవరీ మార్గాన్ని సిద్ధంగా ఉంచుకోండి.

## అభివృద్ధి

[CI వర్క్‌ఫ్లో](../../../.github/workflows/ci.yml) మరియు [Molecule సినారియోలు](../../../molecule) కాన్ఫిగర్ చేయబడిన ధృవీకరణను వివరిస్తాయి. అవి ఉన్నంత మాత్రాన ప్రతి డిస్ట్రిబ్యూషన్‌లో విజయవంతమైన డిప్లాయ్‌మెంట్ జరుగుతుందని నిర్ధారించదు.

## సమ్మతి మరియు లైసెన్స్

మునుపటి README ఈ చెక్‌అవుట్‌లో లేని కంప్లయన్స్-మ్యాపింగ్ డాక్యుమెంట్‌ను ప్రస్తావించింది. ఇక్కడ ఎటువంటి సర్టిఫికేషన్ లేదా ఫ్రేమ్‌వర్క్ సమ్మతి క్లెయిమ్ చేయబడలేదు. ఈ కలెక్షన్ [MIT లైసెన్స్](../../../LICENSE)ను ఉపయోగిస్తుంది; [galaxy.yml](../../../galaxy.yml) ప్యాకేజీ మెటాడేటాను నమోదు చేస్తుంది.

## ఆపరేటింగ్ సిస్టమ్ మద్దతు

nftables బ్యాకెండ్ Ansible core 2.21తో Ubuntu 22.04,
24.04 మరియు 26.04 LTS, Debian 12 మరియు 13, మరియు Rocky Linux 9 మరియు 10లో పరీక్షించబడింది.
Ubuntu 26.04 యొక్క Python 3.14 కోసం core 2.20 లేదా అంతకంటే కొత్త వెర్షన్ ఉన్న మెయింటైన్ చేయబడిన కంట్రోలర్‌ను ఉపయోగించండి.

Molecule కన్వర్జెన్స్‌ను, ఎటువంటి మార్పులు లేకుండా రెండవ రన్‌ను, లోడ్ చేయబడిన రూల్‌సెట్‌ను,
SSH పోర్ట్ 22 అనుమతి నియమాన్ని, కాన్ఫిగరేషన్ అనుమతులను మరియు కృత్రిమంగా సృష్టించిన అప్లై వైఫల్యం తర్వాత
రోల్‌బ్యాక్‌ను తనిఖీ చేస్తుంది. Enterprise Linux `/etc/sysconfig/nftables.conf`ను ఉపయోగిస్తుంది; Debian మరియు
Ubuntu `/etc/nftables.conf`ను ఉపయోగిస్తాయి. `nftables_conf_path` కాన్ఫిగర్ చేయదగినదిగా కొనసాగుతుంది.
ఆ ఓవర్‌రైడ్‌ను ఉపయోగించే ముందు ఏదైనా కస్టమ్ పాత్‌ను లోడ్ చేయడానికి నేటివ్ nftables సర్వీస్‌ను
కాన్ఫిగర్ చేయండి: ఈ రోల్ సర్వీస్‌ను రీస్టార్ట్ చేస్తుంది, లేకపోతే అది తన స్టాక్ ఫైల్‌ను లోడ్ చేసి
ఉద్దేశించిన రూల్‌సెట్‌ను భర్తీ చేయవచ్చు.

Enterprise Linuxలో, భిన్నమైన పాత `/etc/nftables.conf` ఉంటే ప్రమోషన్‌కు ముందే
రోల్ ఆగిపోతుంది. రెండు ఫైళ్లను బ్యాకప్ చేసి, ఆపై ధృవీకరించబడిన మునుపటి కాన్ఫిగరేషన్‌ను
నేటివ్ సర్వీస్ పాత్‌కు తరలించండి. ప్రత్యామ్నాయంగా, పాత పాత్‌ను స్పష్టంగా కొనసాగించి,
తదనుగుణంగా సర్వీస్‌ను కాన్ఫిగర్ చేయండి. `/etc/nftables.conf` వద్ద పాత కాపీని
వదిలివేయవద్దు: అక్కడ భిన్నమైన ఫైల్ ఉంటే తదుపరి రన్‌లలో గార్డ్ ట్రిగ్గర్ అవుతుంది.
అప్‌గ్రేడ్ సమయంలో రోల్‌బ్యాక్ మూలంగా మునుపటి నియమాలు ఎంచుకున్న పాత్‌లో అందుబాటులో ఉంటాయి.

Rocky Linux Enterprise Linux ప్యాకేజీ మరియు సర్వీస్ పాత్‌ను పరీక్షిస్తుంది. ఈ
కంటైనర్ పరీక్షలు RHEL సబ్‌స్క్రిప్షన్‌లను లేదా ప్రొడక్షన్ కెర్నల్‌ను ధృవీకరించవు.
విస్తృత మద్దతు లేని Ubuntu 20.04, Debian 11 మరియు EL8 యొక్క పాత Python
ప్రస్తుత పరీక్ష మ్యాట్రిక్స్ పరిధికి వెలుపల ఉన్నాయి.
