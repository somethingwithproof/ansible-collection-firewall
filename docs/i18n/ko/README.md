<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · **한국어** · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · **한국어** · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

호스트 방화벽 정책을 정의하고 nftables 구성을 적용하기 위한 Ansible 컬렉션입니다. 주소 그룹을 사용하여 정책을 생성된 규칙 집합과 분리합니다.

## 구현 및 범위

- [firewall](../../../roles/firewall/tasks/main.yml)은 공개 기본값과 백엔드 검증을 담당하며, [policy](../../../roles/policy/tasks/main.yml)는 이를 가져와 레거시 진입점을 유지합니다.
- [nftables](../../../roles/nftables/tasks/main.yml)는 nftables를 설치하고, 구성을 스테이징한 뒤 `nft -c -f`로 검증하고 `nft -f`로 적용합니다.
- 적용 블록은 실패 시 이전 구성이 존재했던 경우 이를 복원하려고 시도합니다. 백업 가용성과 복구 접근 경로 확보는 운영상 필수 요건으로 남아 있습니다.
- 지원되는 정책 필드는 [템플릿](../../../roles/nftables/templates/nftables.conf.j2)과 [기본값](../../../roles/policy/defaults/main.yml)에 정의되어 있습니다.

이 컬렉션은 `firewall`, `policy`, `nftables`를 패키징하며, 이 중 `nftables`만 방화벽 백엔드를 구현합니다. 정책 검증에서는 iptables와 firewalld 백엔드 이름도 허용하지만, 이에 해당하는 백엔드 역할은 여기에 포함되어 있지 않습니다. 템플릿에 포함된 SSH 규칙이 잠금(lockout)을 완전히 방지해 주지는 않습니다. 검증 전용(validate-only) 모드에서도 패키지를 설치하고 구성 및 서비스 작업을 수행하므로 부작용 없는 드라이 런이 아닙니다.

## 빠른 시작

이 체크아웃에서 컬렉션을 빌드하고 설치합니다:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

파일 이름은 [galaxy.yml](../../../galaxy.yml)의 버전을 따릅니다. 해당 버전이 변경된 경우 빌드에서 생성된 파일 이름을 사용하세요.

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

원격 호스트에 적용하기 전에 테스트 호스트에서 생성된 정책을 검토하세요. 방화벽 정책을 변경할 때는 독립적인 복구 경로를 확보해 두어야 합니다.

## 개발

구성된 검증 절차는 [CI 워크플로](../../../.github/workflows/ci.yml)와 [Molecule 시나리오](../../../molecule)에 정의되어 있습니다. 이들이 존재한다고 해서 모든 배포판에서의 성공적인 배포가 보장되는 것은 아닙니다.

## 규정 준수 및 라이선스

이전 README에서는 이 체크아웃에 존재하지 않는 규정 준수 매핑 문서를 참조했습니다. 여기서는 어떠한 인증이나 프레임워크 준수도 주장하지 않습니다. 이 컬렉션은 [MIT 라이선스](../../../LICENSE)를 따르며, 패키지 메타데이터는 [galaxy.yml](../../../galaxy.yml)에 기록되어 있습니다.

## 운영체제 지원

nftables 백엔드는 Ubuntu 22.04, 24.04 및 26.04 LTS, Debian 12 및 13, 그리고 Rocky Linux 9 및 10에서 Ansible core 2.21로 테스트되었습니다. Ubuntu 26.04의 Python 3.14를 사용하려면 core 2.20 이상을 갖춘 유지 관리되는 컨트롤러를 사용하세요.

Molecule은 수렴성, 변경 사항이 없는 두 번째 실행, 로드된 규칙 집합, SSH 포트 22 허용 규칙, 구성 권한, 그리고 의도적으로 주입한 적용 실패 이후의 롤백을 검증합니다. Enterprise Linux는 `/etc/sysconfig/nftables.conf`를 사용하며, Debian과 Ubuntu는 `/etc/nftables.conf`를 사용합니다. `nftables_conf_path`는 계속 설정할 수 있습니다. 이 재정의 설정을 사용하기 전에 네이티브 nftables 서비스가 사용자 지정 경로를 로드하도록 구성하세요. 그렇지 않으면 역할이 서비스를 재시작할 때 기본 파일을 로드하여 의도한 규칙 집합을 덮어쓸 수 있습니다.

Enterprise Linux에서 내용이 다른 레거시 `/etc/nftables.conf` 파일이 존재하면 역할은 구성을 적용하기 전에 실행을 중지합니다. 두 파일을 모두 백업한 다음, 검증된 이전 구성을 네이티브 서비스 경로로 이동하세요. 또는 레거시 경로를 명시적으로 유지하고 그에 맞게 서비스를 구성할 수도 있습니다. `/etc/nftables.conf`에 오래된 사본을 남겨 두지 마세요. 내용이 다른 파일이 남아 있으면 이후 실행 시 보호 장치가 동작합니다. 업그레이드 중에는 선택한 경로에 있는 이전 규칙이 롤백 소스로 유지됩니다.

Rocky Linux는 Enterprise Linux 패키지 및 서비스 경로를 검증합니다. 이러한 컨테이너 테스트는 RHEL 구독이나 프로덕션 커널을 인증하지 않습니다. 확장 지원이 없는 Ubuntu 20.04, Debian 11, 그리고 구버전 Python을 사용하는 EL8은 현재 테스트 매트릭스에서 제외되어 있습니다.
