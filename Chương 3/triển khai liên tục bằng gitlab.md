** Triển khai ứng dụng từ CI/CD pipeline

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

build:
  stage: build
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py check

test:
  stage: test
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py test taskManager

release:
  stage: release
  before_script:
   - echo $CI_REGISTRY_PASSWORD | docker login -u $CI_REGISTRY_USER --password-stdin $CI_REGISTRY
  script:
   - docker build -t $CI_REGISTRY_IMAGE .  # Build the application into Docker image
   - docker push $CI_REGISTRY_IMAGE        # Push the image into registry

integration:
  stage: integration
  script:
    - echo "This is an integration step"
    - exit 1
  allow_failure: true # Even if the job fails, continue to the next stages

prod:
  stage: prod
  image: kroniak/ssh-client:3.6
  environment: production
  only:
      - main
  before_script:
   - mkdir -p ~/.ssh
   - echo "$PROD_SSH_PRIVKEY" > ~/.ssh/id_rsa
   - chmod 600 ~/.ssh/id_rsa
   - eval "$(ssh-agent -s)"
   - ssh-add ~/.ssh/id_rsa
   - ssh-keyscan -H $PROD_HOSTNAME >> ~/.ssh/known_hosts
  script:
   - echo
   - |
      ssh $PROD_USERNAME@$PROD_HOSTNAME << EOF
        docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
        docker rm -f django.nv
        docker run -d --name django.nv -p 8000:8000 $CI_REGISTRY_IMAGE
      EOF
```

- Bước 1: Chuẩn bị server production/staging

- Bước 2: Lấy SSH private key trên server

- Bước 3: Tạo CI/CD Variables trong GitLab

- Bước 4: Sửa file .gitlab-ci.yml

- Bước 5: Build Stage

- Bước 6: Test Stage

- Bước 7: Release Stage

- Bước 8: Integration Stage

- Bước 9: Prod Stage

- Bước 10: SSH vào server từ Pipeline

- Bước 11: Trên server, đăng nhập Registry

- Bước 12: Xóa container cũ

- Bước 13: Chạy container mới

- Bước 14: Kiểm tra ứng dụng
