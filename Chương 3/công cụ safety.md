Safety là công cụ Dependency Vulnerability Scanning, dùng để kiểm tra xem các thư viện Python mà dự án đang dùng có CVE/lỗ hổng đã biết hay không.

```
services:
  - docker:dind

stages:
  - build
  - test
  - release
  - preprod
  - integration
  - prod

job1:
  stage: build
  script:
    - |
      cat > .safety-policy.yml <<EOF
      security:
        ignore-vulnerabilities: {}
      EOF
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r /src/requirements.txt --json
```

`docker run --rm -v $(pwd):/src hysnsec/safety check -r /src/requirements.txt --json`

- `--rm`:	chạy xong xóa container luôn

- `-v $(pwd):/src`:	chia sẻ thư mục hiện tại của GitLab Runner vào thư mục /src trong container

- `hysnsec/safety`:	Image Docker chứa công cụ Safety

- `check`: Safety bắt đầu quét

- `-r /src/requirements.txt`:	đọc các package từ file requirements.txt

- `--json`:	xuất kết quả dưới dạng JSON
