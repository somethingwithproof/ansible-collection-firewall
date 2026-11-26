<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Tiếng Việt** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · **Tiếng Việt** · [Bahasa Indonesia](../id/README.md)
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

Một bộ sưu tập Ansible dùng để mô tả chính sách tường lửa trên máy chủ và áp dụng cấu hình nftables. Các nhóm địa chỉ giúp tách biệt chính sách khỏi bộ quy tắc được tạo ra.

## Triển khai và phạm vi giới hạn

- [firewall](../../../roles/firewall/tasks/main.yml) quản lý các giá trị mặc định công khai và kiểm tra hợp lệ backend; [policy](../../../roles/policy/tasks/main.yml) duy trì điểm vào cũ bằng cách import vai trò này.
- [nftables](../../../roles/nftables/tasks/main.yml) cài đặt nftables, chuẩn bị cấu hình tạm, kiểm tra hợp lệ bằng `nft -c -f` và áp dụng bằng `nft -f`.
- **`nftables_backup: true`** — Khối tác vụ áp dụng sẽ cố gắng khôi phục cấu hình trước đó sau khi xảy ra lỗi nếu đã có cấu hình cũ. Việc đảm bảo sẵn sàng bản sao lưu và quyền truy cập khôi phục vẫn là yêu cầu vận hành.
- [Mẫu cấu hình](../../../roles/nftables/templates/nftables.conf.j2) và [giá trị mặc định](../../../roles/firewall/defaults/main.yml) định nghĩa các trường chính sách được hỗ trợ.

Bộ sưu tập này đóng gói `firewall`, `policy` và `nftables`; chỉ có `nftables` thực sự triển khai backend tường lửa. Tên backend cho iptables và firewalld vẫn được chấp nhận trong bước kiểm tra hợp lệ chính sách, nhưng không có vai trò backend tương ứng nào được cung cấp tại đây. Một quy tắc SSH trong mẫu cấu hình không đảm bảo tránh bị khóa quyền truy cập. Chế độ chỉ kiểm tra (validate-only) vẫn cài đặt các gói và thực hiện các tác vụ cấu hình/dịch vụ; đây không phải là chế độ chạy thử hoàn toàn không gây tác dụng phụ.

## Bắt đầu nhanh

Đóng gói và cài đặt bộ sưu tập từ bản sao mã nguồn này:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Tên tệp tuân theo phiên bản trong [galaxy.yml](../../../galaxy.yml). Hãy sử dụng tên tệp được tạo ra sau khi đóng gói nếu phiên bản đó thay đổi.

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
        office: ["203.0.113.0/24"]  # Thay bằng mạng văn phòng của bạn (CIDR)
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

Hãy rà soát chính sách được tạo ra trên một máy chủ thử nghiệm trước khi áp dụng cho các máy chủ từ xa. Luôn giữ một phương án khôi phục độc lập khi thay đổi chính sách tường lửa.

## Phát triển

[Quy trình CI](../../../.github/workflows/ci.yml) và [các kịch bản Molecule](../../../molecule) mô tả các bước kiểm tra đã được cấu hình. Sự hiện diện của chúng không đồng nghĩa với việc đảm bảo triển khai thành công trên mọi bản phân phối.

## Tuân thủ và giấy phép

Tệp README trước đây có dẫn chiếu đến một tài liệu ánh xạ tuân thủ nhưng tài liệu đó không tồn tại trong bản mã nguồn này. Không có bất kỳ chứng nhận hay tuyên bố tuân thủ khung tiêu chuẩn nào được đưa ra ở đây. Bộ sưu tập sử dụng [giấy phép MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) lưu trữ siêu dữ liệu gói.

## Hỗ trợ hệ điều hành

Backend nftables được kiểm thử với Ansible core 2.21 trên Ubuntu 22.04, 24.04 và 26.04 LTS, Debian 12 và 13, cùng với Rocky Linux 9 và 10. Hãy sử dụng máy điều khiển còn được bảo trì với core 2.20 trở lên cho Python 3.14 trên Ubuntu 26.04.

Molecule kiểm tra khả năng hội tụ, lần chạy thứ hai không phát sinh thay đổi, bộ quy tắc đã nạp, quy tắc cho phép SSH cổng 22, quyền truy cập tệp cấu hình và khả năng hoàn tác sau khi chèn lỗi áp dụng. Enterprise Linux sử dụng `/etc/sysconfig/nftables.conf`; Debian và Ubuntu sử dụng `/etc/nftables.conf`. Biến `nftables_conf_path` vẫn có thể tùy chỉnh. Hãy cấu hình dịch vụ nftables gốc để nạp đường dẫn tùy chỉnh trước khi sử dụng giá trị ghi đè đó: vai trò sẽ khởi động lại dịch vụ, và nếu không được cấu hình trước, dịch vụ sẽ nạp tệp mặc định và có thể ghi đè bộ quy tắc mong muốn.

Trên Enterprise Linux, một tệp cũ `/etc/nftables.conf` có nội dung khác biệt sẽ làm dừng vai trò trước bước kích hoạt. Hãy sao lưu cả hai tệp, sau đó di chuyển cấu hình trước đó đã được kiểm tra hợp lệ sang đường dẫn dịch vụ gốc. Ngoài ra, bạn có thể giữ nguyên đường dẫn cũ một cách tường minh và cấu hình dịch vụ tương ứng. Đừng để lại bản sao cũ lỗi thời tại `/etc/nftables.conf`: một tệp có nội dung khác biệt tại đó sẽ kích hoạt cơ chế bảo vệ ở các lần chạy sau. Các quy tắc trước đó vẫn được giữ tại đường dẫn đã chọn làm nguồn hoàn tác trong quá trình nâng cấp.

Rocky Linux kiểm chứng quy trình gói và dịch vụ của Enterprise Linux. Các bài kiểm thử trong container này không chứng nhận cho gói thuê bao RHEL hay nhân hệ điều hành trên môi trường sản xuất. Ubuntu 20.04 không có hỗ trợ mở rộng, Debian 11 và phiên bản Python cũ trên EL8 nằm ngoài ma trận kiểm thử hiện tại.
