* Các loại máy trong lab

DevSecOps: Máy này hoạt động như một máy dành cho nhà phát triển/kỹ sư bảo mật và được trang bị nhiều công cụ DevSecOps.

GitLab: Máy này có hệ thống quản lý mã nguồn (SCM), hệ thống CI/CD và các công cụ khác.

GitLab runner: Máy này hoạt động như một tác nhân trong hệ thống điều khiển/tác nhân và chạy các lệnh từ máy chủ chính GitLab.

Production: Máy này đóng vai trò là máy production.

Vulnerability Management: Máy này chạy phần mềm để quản lý các lỗ hổng được phát hiện trong các lần quét bảo mật hàng ngày.

Other machines: Các công cụ khác như Docker Registry, SonarQube, Jenkins, v.v., trong các bài tập cụ thể.

* Các loại máy trong lab, tên miền và thời gian hoạt động

DevSecOps-Box / `devsecops-box-xxx` / Dừng lại khi đồng hồ đếm giờ của lab kết thúc.

Git server / `gitlab-ce-xxx` /	2 giờ

CI/CD	/ `gitlab-ce-xxx` /	2 giờ

Prod	/ `prod-xxx`	/ 2 giờ

Vulnerability Management	/ `dojo-xxx` /	2 giờ

xxx là một số ngẫu nhiên duy nhất dành cho mỗi người (Mã số máy (Machine ID)).

* 1 số câu lệnh

`hostname` lệnh hiển thị tên máy chủ của hệ thống, là tên được gán cho một máy tính hoặc thiết bị trên mạng.

`ping -c 3 prod-xxx` lệnh ping các máy khác trong lab.

`ssh root@prod-xxx` lệnh đăng nhập vào các máy khác trong lab.

`exit` lệnh thoát.

* Truy cập các dịch vụ web và cổng đã triển khai

Production Machine

- Link	`https://prod-xxx.lab.practical-devsecops.training`

- Username	`admin`

- Password	`admin`

Vulnerability Management Machine

- Link	`https://dojo-xxx.lab.practical-devsecops.training`

- Username	`root`

- Password	`pdso-training`


