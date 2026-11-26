<!-- translations:start -->
**Languages:** [English](../../../README.md) · **தமிழ்** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · **தமிழ்** · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

ஹோஸ்ட் ஃபயர்வால் கொள்கையை விவரிக்கவும் nftables கட்டமைப்பைப் பயன்படுத்தவும் உதவும் ஒரு Ansible தொகுப்பு. முகவரி குழுக்கள் கொள்கையை உருவாக்கப்படும் விதித் தொகுப்பிலிருந்து தனித்து வைக்கின்றன.

## செயல்படுத்தல் மற்றும் எல்லைகள்

- [firewall](../../../roles/firewall/tasks/main.yml) பொது இயல்புநிலைகளையும் பின்புலச் சரிபார்ப்பையும் நிர்வகிக்கிறது; [policy](../../../roles/policy/tasks/main.yml) அதை இறக்குமதி செய்வதன் மூலம் பழைய நுழைவுப் புள்ளியைப் பாதுகாக்கிறது.
- [nftables](../../../roles/nftables/tasks/main.yml) nftables-ஐ நிறுவுகிறது, கட்டமைப்பைத் தயார்படுத்துகிறது, `nft -c -f` மூலம் அதைச் சரிபார்க்கிறது, மேலும் `nft -f` மூலம் பயன்படுத்துகிறது.
- **`nftables_backup: true`** — பயன்பாட்டுத் தொகுதி தோல்வியடைந்த பிறகு, முந்தைய கட்டமைப்பு இருந்தால் அதை மீட்டெடுக்க முயற்சிக்கிறது. காப்புப்பிரதி கிடைப்பதும் மீட்பு அணுகலும் செயல்பாட்டுத் தேவைகளாகவே உள்ளன.
- [வார்ப்புரு](../../../roles/nftables/templates/nftables.conf.j2) மற்றும் [இயல்புநிலைகள்](../../../roles/firewall/defaults/main.yml) ஆதரிக்கப்படும் கொள்கைப் புலங்களை வரையறுக்கின்றன.

இந்தத் தொகுப்பு `firewall`, `policy`, மற்றும் `nftables` ஆகியவற்றை உள்ளடக்கியுள்ளது; `nftables` மட்டுமே ஃபயர்வால் பின்புலத்தைச் செயல்படுத்துகிறது. iptables மற்றும் firewalld ஆகியவற்றிற்கான பின்புலப் பெயர்கள் கொள்கைச் சரிபார்ப்பால் ஏற்றுக்கொள்ளப்படுகின்றன, ஆனால் அதற்கான பின்புலப் பாத்திரங்கள் எதுவும் இதில் வழங்கப்படவில்லை. வார்ப்புருவில் உள்ள SSH விதி பூட்டப்படுவதற்கு (lockout) எதிரான உத்தரவாதம் அல்ல. சரிபார்ப்பு-மட்டும் பயன்முறை இன்னும் தொகுப்புகளை நிறுவுகிறது மற்றும் கட்டமைப்பு/சேவைப் பணிகளைச் செய்கிறது; இது பக்கவிளைவுகள் இல்லாத உலர் இயக்கம் அல்ல.

## விரைவுத் தொடக்கம்

இந்தச் சரிபார்ப்புப் பிரதியிலிருந்து தொகுப்பை உருவாக்கி நிறுவவும்:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

கோப்புப் பெயர் [galaxy.yml](../../../galaxy.yml)-இல் உள்ள பதிப்பைப் பின்பற்றுகிறது. அந்தப் பதிப்பு மாறினால், உருவாக்கத்தின்போது பெறப்படும் கோப்புப் பெயரைப் பயன்படுத்தவும்.

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
        office: ["203.0.113.0/24"]  # உங்கள் அலுவலக வலையமைப்பால் மாற்றவும் (CIDR)
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

தொலைநிலை ஹோஸ்ட்களில் பயன்படுத்துவதற்கு முன், உருவாக்கப்பட்ட கொள்கையை ஒரு சோதனை ஹோஸ்டில் மதிப்பாய்வு செய்யவும். ஃபயர்வால் கொள்கையை மாற்றும்போது தனித்தனி மீட்புப் பாதையை வைத்திருக்கவும்.

## மேம்பாடு

[CI பணிப்பாய்வு](../../../.github/workflows/ci.yml) மற்றும் [Molecule சூழ்நிலைகள்](../../../molecule) ஆகியவை கட்டமைக்கப்பட்ட சரிபார்ப்பை விவரிக்கின்றன. இவை இருப்பது ஒவ்வொரு விநியோகத்திலும் வெற்றிகரமான வரிசைப்படுத்தலை உறுதிப்படுத்தாது.

## இணக்கம் மற்றும் உரிமம்

முந்தைய README-இல் குறிப்பிடப்பட்டிருந்த இணக்க-வரைபட ஆவணம் இந்தப் பிரதியில் இல்லை. எந்தவொரு சான்றிதழும் அல்லது கட்டமைப்பு இணக்கமும் இங்கு கோரப்படவில்லை. இந்தத் தொகுப்பு [MIT உரிமத்தைப்](../../../LICENSE) பயன்படுத்துகிறது; [galaxy.yml](../../../galaxy.yml) தொகுப்பு மெட்டாடேட்டாவைப் பதிவு செய்கிறது.

## இயக்க முறைமை ஆதரவு

nftables பின்புலம் Ubuntu 22.04, 24.04 மற்றும் 26.04 LTS, Debian 12 மற்றும் 13, மற்றும் Rocky Linux 9 மற்றும் 10 ஆகியவற்றில் Ansible core 2.21 உடன் சோதிக்கப்பட்டுள்ளது. Ubuntu 26.04-இன் Python 3.14-க்கு core 2.20 அல்லது அதற்கு மேற்பட்ட பதிப்பைக் கொண்ட பராமரிக்கப்படும் கட்டுப்படுத்தியைப் பயன்படுத்தவும்.

Molecule ஒருங்கிணைப்பு (convergence), மாற்றங்கள் இல்லாத இரண்டாவது இயக்கம், ஏற்றப்பட்ட விதித் தொகுப்பு, SSH போர்ட் 22 அனுமதி விதி, கட்டமைப்பு அனுமதிகள் மற்றும் திணிக்கப்பட்ட பயன்பாட்டுத் தோல்விக்குப் பிந்தைய பின்வாங்கல் (rollback) ஆகியவற்றைச் சரிபார்க்கிறது. Enterprise Linux `/etc/sysconfig/nftables.conf`-ஐப் பயன்படுத்துகிறது; Debian மற்றும் Ubuntu ஆகியவை `/etc/nftables.conf`-ஐப் பயன்படுத்துகின்றன. `nftables_conf_path` தொடர்ந்து கட்டமைக்கக்கூடியதாக உள்ளது. அந்த மேலெழுதலைப் பயன்படுத்துவதற்கு முன்பு, எந்தவொரு தனிப்பயன் பாதையையும் ஏற்றுமாறு இயல்பான nftables சேவையைக் கட்டமைக்கவும்: இந்தப் பாத்திரம் சேவையை மறுதொடக்கம் செய்கிறது; இல்லையெனில் அது தனது இயல்புநிலைக் கோப்பை ஏற்றி, உத்தேசித்த விதித் தொகுப்பை மாற்றிவிடக்கூடும்.

Enterprise Linux-இல், வேறுபடும் பழைய `/etc/nftables.conf` கோப்பு மாற்றத்தை நடைமுறைப்படுத்துவதற்கு முன்பே பாத்திரத்தை நிறுத்திவிடும். இரண்டு கோப்புகளையும் காப்புப்பிரதி எடுத்து, பின்னர் சரிபார்க்கப்பட்ட முந்தைய கட்டமைப்பை இயல்பான சேவைப் பாதைக்கு நகர்த்தவும். மாற்றாக, பழைய பாதையை வெளிப்படையாகத் தக்கவைத்து, அதற்கேற்ப சேவையைக் கட்டமைக்கவும். `/etc/nftables.conf`-இல் பழைய நகலை விட்டுவைக்க வேண்டாம்: அங்குள்ள வேறுபடும் கோப்பு அடுத்தடுத்த இயக்கங்களில் பாதுகாப்புத் தடையைத் தூண்டும். மேம்படுத்தலின்போது பின்வாங்கல் மூலமாகத் தேர்ந்தெடுக்கப்பட்ட பாதையில் முந்தைய விதிகள் தொடர்ந்து கிடைக்கும்.

Rocky Linux ஆனது Enterprise Linux தொகுப்பு மற்றும் சேவைப் பாதையைச் சோதிக்கிறது. இந்தக் கொள்கலன் சோதனைகள் RHEL சந்தாக்களையோ அல்லது தயாரிப்பு கர்னலையோ சான்றளிப்பதில்லை. நீட்டிக்கப்பட்ட ஆதரவு இல்லாத Ubuntu 20.04, Debian 11 மற்றும் EL8-இன் பழைய Python ஆகியவை தற்போதைய சோதனை அணியின் வரம்பிற்கு அப்பாற்பட்டவை.
