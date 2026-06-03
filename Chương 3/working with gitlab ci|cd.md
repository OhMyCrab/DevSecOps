** YAML Syntax

Các tệp YAML có thể tùy chọn bắt đầu bằng ba dấu gạch ngang `---`.

Dictionary (cặp khóa/giá trị) được biểu diễn theo định dạng khóa: giá trị. Hãy nhớ thêm một dấu cách sau dấu hai chấm. GitLab CI/CD sử dụng định dạng này để cấu hình các thiết lập riêng lẻ trong một job. Ví dụ:

```
  stage: test
  image: node:alpine3.10
  script: echo "hello world"
```

Dictionary có thể chứa các dictionary hoặc danh sách khác bên trong nó. Điều này cho phép tạo ra các cấu trúc lồng nhau trong YAML.

```
stages:
  - build        # Giai đoạn xây dựng
  - test         # Giai đoạn thử nghiệm
  - integration  # Giai đoạn tích hợp
  - prod         # Giai đoạn triển khai (production)

job1:
  stage: build
  script:
    - echo "This is a build step."

job2:
  stage: test
  script:
    - echo "This is a test step."
    - exit 1     # Mã thoát khác 0, khiến job bị thất bại

job3:
  stage: integration
  script:
    - echo "This is an integration step."

job4:
  stage: prod
  script:
    - echo "This is a deploy step."
```

