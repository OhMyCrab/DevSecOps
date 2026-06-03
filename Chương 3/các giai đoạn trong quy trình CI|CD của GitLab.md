build stage là giai đoạn đầu tiên của quy trình phát triển phần mềm. Quá trình này tạo ra một phiên bản phần mềm có thể chạy được gọi là bản dựng (build). Giai đoạn này đảm bảo mã nguồn có thể được chuyển đổi thành một ứng dụng hoạt động.

test stage là giai đoạn đảm bảo rằng những thay đổi mới không làm hỏng chức năng hiện có và đáp ứng các tiêu chuẩn chất lượng cần thiết.

integration stage là giai đoạn kết hợp và xác thực chức năng của mã mới với mã hoặc dịch vụ hiện có.

deploy stage là giai đoạn chuyển ứng dụng từ môi trường phát triển hoặc thử nghiệm sang môi trường thực tế, nơi người dùng có thể truy cập

```
services:
  - docker:dind

stages:
  - build
  - test
  - integration
  - deploy

django_build:
  stage: build
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py check

django_test:
  stage: test
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py test taskManager

django_integration:
  stage: integration
  script:
    - echo "This is an integration step"
    - exit 1
  allow_failure: true # Even if the job fails, continue to the next stages

django_deployment:
  stage: deploy
  script:
    - echo "This is a deploy step."
  when: manual # Continuous Delivery
```
