CINC Auditor là một framework mã nguồn mở dùng để kiểm tra và audit ứng dụng và hạ tầng.

Tải xuống gói Debian CinC Auditor từ trang web của CINC.

`wget https://omnitruck.cinc.sh/install.sh`

`bash install.sh -P cinc-auditor -v 6` - lệnh cài đặt CinC Auditor

`echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config`

`cinc-auditor exec https://github.com/dev-sec/linux-baseline.git -t ssh://root@prod-8t8ba53v -i ~/.ssh/id_rsa --chef-license accept`
