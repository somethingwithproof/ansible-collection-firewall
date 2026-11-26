<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Türkçe** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · **Türkçe** · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Ana bilgisayar güvenlik duvarı politikasını tanımlamak ve nftables yapılandırmasını uygulamak için bir Ansible koleksiyonu. Adres grupları, politikayı oluşturulan kural kümesinden ayrı tutar.

## Uygulama ve sınırlar

- [firewall](../../../roles/firewall/tasks/main.yml) genel varsayılanları ve arka uç doğrulamasını yönetir; [policy](../../../roles/policy/tasks/main.yml) bunu içe aktararak eski giriş noktasını korur.
- [nftables](../../../roles/nftables/tasks/main.yml) nftables paketini kurar, yapılandırmayı hazırlar, `nft -c -f` ile doğrular ve `nft -f` ile uygular.
- Uygulama bloğu, bir hata oluştuğunda mevcutsa önceki yapılandırmayı geri yüklemeye çalışır. Yedeklerin erişilebilirliği ve kurtarma erişimi operasyonel gereksinimler olarak kalır.
- [şablon](../../../roles/nftables/templates/nftables.conf.j2) ve [varsayılanlar](../../../roles/policy/defaults/main.yml) desteklenen politika alanlarını tanımlar.

Koleksiyon `firewall`, `policy` ve `nftables` rollerini paketler; yalnızca `nftables` bir güvenlik duvarı arka ucu uygular. iptables ve firewalld için arka uç adları politika doğrulaması tarafından kabul edilir, ancak bunlara karşılık gelen arka uç rolleri burada sunulmaz. Şablondaki bir SSH kuralı, sistemden kilitlenmeye karşı bir garanti değildir. Yalnızca doğrulama modu yine de paketleri kurar ve yapılandırma/servis görevlerini yürütür; yan etkisiz bir deneme çalıştırması değildir.

## Hızlı başlangıç

Koleksiyonu bu çalışma kopyasından derleyin ve kurun:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Dosya adı [galaxy.yml](../../../galaxy.yml) dosyasındaki sürümü izler. Bu sürüm değişirse derleme tarafından üretilen dosya adını kullanın.

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

Uzak ana bilgisayarlara uygulamadan önce oluşturulan politikayı bir test ana bilgisayarında inceleyin. Güvenlik duvarı politikasını değiştirirken bağımsız bir kurtarma yolu bulundurun.

## Geliştirme

[CI iş akışı](../../../.github/workflows/ci.yml) ve [Molecule senaryoları](../../../molecule) yapılandırılmış doğrulamayı tanımlar. Bunların varlığı her dağıtımda başarılı kurulum anlamına gelmez.

## Uyumluluk ve lisans

Önceki README, bu çalışma kopyasında bulunmayan bir uyumluluk eşleştirme belgesine atıfta bulunuyordu. Burada herhangi bir sertifikasyon veya çerçeve uyumluluğu iddia edilmemektedir. Koleksiyon [MIT lisansı](../../../LICENSE) ile lisanslanmıştır; paket üst verileri [galaxy.yml](../../../galaxy.yml) dosyasında kayıtlıdır.

## İşletim sistemi desteği

nftables arka ucu, Ubuntu 22.04, 24.04 ve 26.04 LTS, Debian 12 ve 13 ile Rocky Linux 9 ve 10 üzerinde Ansible core 2.21 ile test edilmiştir. Ubuntu 26.04'ün Python 3.14 sürümü için core 2.20 veya daha yeni bir sürüme sahip desteklenen bir denetleyici kullanın.

Molecule; yakınsamayı, değişiklik olmadan ikinci bir çalıştırmayı, yüklenen kural kümesini, SSH 22 numaralı bağlantı noktası izin kuralını, yapılandırma izinlerini ve enjekte edilmiş bir uygulama hatasının ardından geri almayı kontrol eder. Enterprise Linux `/etc/sysconfig/nftables.conf` yolunu; Debian ve Ubuntu ise `/etc/nftables.conf` yolunu kullanır. `nftables_conf_path` yapılandırılabilir durumdadır. Bu geçersiz kılmayı kullanmadan önce yerel nftables servisini özel yolu yükleyecek şekilde yapılandırın: rol servisi yeniden başlatır; aksi takdirde servis varsayılan dosyasını yükler ve amaçlanan kural kümesinin üzerine yazabilir.

Enterprise Linux üzerinde farklılık gösteren eski bir `/etc/nftables.conf` dosyası, yapılandırmayı devreye almadan önce rolü durdurur. Her iki dosyayı da yedekleyin, ardından doğrulanmış önceki yapılandırmayı yerel servis yoluna taşıyın. Alternatif olarak, eski yolu açıkça koruyun ve servisi buna göre yapılandırın. `/etc/nftables.conf` konumunda güncel olmayan bir kopya bırakmayın: orada farklılık gösteren bir dosya sonraki çalıştırmalarda korumayı tetikler. Önceki kurallar, bir yükseltme sırasında geri alma kaynağı olarak seçilen yolda kullanılabilir durumda kalır.

Rocky Linux, Enterprise Linux paket ve servis yolunu test eder. Bu kapsayıcı testleri RHEL aboneliklerini veya bir üretim çekirdeğini sertifikalandırmaz. Genişletilmiş desteği olmayan Ubuntu 20.04, Debian 11 ve EL8'in daha eski Python sürümü mevcut test matrisinin dışındadır.
