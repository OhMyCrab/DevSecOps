Kỳ thi CDP chính thức sẽ bao gồm 5 thử thách với tổng điểm là 100.

- Phân tích mã nguồn tĩnh (SAST)

- Phân tích động (DAST)

- Phân tích thành phần phần mềm (SCA)

- Cấu hình và tích hợp bảo mật vào pipeline CI/CD

- Bảo mật và gia cố (hardening) cho Infrastructure as Code (IaC)

- Compliance as Code

- Quản lý lỗ hổng (Vulnerability Management)

- Các chủ đề khác trong khóa học

Hướng dẫn từng bước (Step-by-step instructions)

1. Đăng nhập GitLab

2. Mở repository target

3. Chỉnh sửa file .gitlab-ci.yml

4. Thêm job semgrep_scan

5. Commit thay đổi

6. Chạy pipeline

7. Tải artifact

2. Các file đã sử dụng

Ví dụ:

- .gitlab-ci.yml

- roles/

- playbook.yml

- Dockerfile

- docker-compose.yml

- checkov.yml

- semgrep.yml

- inspec.yml
 
- Nếu có chỉnh sửa file, nên đưa nội dung hoặc phần thay đổi vào báo cáo.

3. Ảnh chụp màn hình (Screenshots)

Ví dụ:

- Pipeline chạy thành công

- Kết quả scan

- Job log

- Artifact được tạo

- Merge Request

- Dashboard của tool

4. Kết quả / Output

- Nên cung cấp kết quả ở định dạng máy có thể đọc được (machine-readable format), chẳng hạn:

- `JSON`

- `XML`

Ví dụ:

```
{
  "vulnerabilities": 5,
  "critical": 1,
  "high": 2,
  "medium": 2
}
```

hoặc file:

```
semgrep-results.json
trivy-results.json
safety-results.json
checkov-results.xml
```

| Thành phần                   | Mô tả                                                                                                                                         |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **DevSecOps Box**            | Máy này đóng vai trò là máy của lập trình viên/kỹ sư bảo mật và đã được cài sẵn nhiều công cụ DevSecOps.                                      |
| **GitLab**                   | Máy này chứa hệ thống quản lý mã nguồn (**SCM - Source Code Management**), hệ thống **CI/CD** và nhiều công cụ khác.                          |
| **GitLab Runner**            | Máy này đóng vai trò là máy tác nhân (**agent**) trong mô hình kiến trúc controller/agent và thực thi các lệnh được GitLab Master giao xuống. |
| **Production**               | Máy này đóng vai trò là môi trường sản xuất (**Production Environment**).                                                                     |
| **Vulnerability Management** | Máy này chạy phần mềm quản lý lỗ hổng bảo mật, dùng để quản lý các lỗ hổng được phát hiện trong quá trình quét bảo mật hằng ngày.             |


| Tên hệ thống                 | Domain                   |
| ---------------------------- | ------------------------ |
| **DevSecOps-Box**            | `devsecops-box-8t8ba53v` |
| **Git Server**               | `gitlab-ce-8t8ba53v`     |
| **CI/CD**                    | `gitlab-ce-8t8ba53v`     |
| **Prod**                     | `prod-8t8ba53v`          |
| **Vulnerability Management** | `dojo-8t8ba53v`          |

- Thông tin truy cập môi trường GitLab của lab:

| Thuộc tính | Giá trị                                                       |
| ---------- | ------------------------------------------------------------- |
| Link       | `https://gitlab-ce-8t8ba53v.lab.practical-devsecops.training` |
| Username   | `root`                                                        |
| Password   | `pdso-training`                                               |

- Môi trường Production giả lập để triển khai ứng dụng hoặc kiểm tra kết quả sau khi pipeline chạy:

| Thuộc tính | Giá trị                                                  |
| ---------- | -------------------------------------------------------- |
| Link       | `https://prod-8t8ba53v.lab.practical-devsecops.training` |
| Username   | `admin`                                                  |
| Password   | `admin`                                                  |

- Hệ thống quản lý lỗ hổng (DefectDojo).

| Thuộc tính | Giá trị                                                   |
| ---------- | --------------------------------------------------------- |
| Link       | `https://dojo-8t8ba53v.lab.practical-devsecops.training/` |
| Username   | `root`                                                    |
| Password   | `pdso-training`                                           |

** Script Upload DefectDojo

Có thể tải một script đơn giản để upload kết quả lên DefectDojo bằng lệnh sau:

`curl https://gitlab.practical-devsecops.training/-/snippets/28/raw -o upload-results.py`

`curl URL -o upload-results.py`

`curl`: tải nội dung từ URL.

`https://gitlab.practical-devsecops.training/-/snippets/28/raw`: file script Python được lưu trên GitLab.

`-o upload-results.py`: lưu nội dung tải về thành file upload-results.py.

Sau khi chạy lệnh, thư mục hiện tại sẽ có: `upload-results.py`

