`rules` là cú pháp YAML dành cho GitLab CI để include hoặc exclude các công việc trong pipeline. Nó thay thế các mệnh đề `only` và `except`. Sử dụng cú pháp này có thể kiểm soát thời điểm một công việc sẽ được kích hoạt trong pipeline dựa trên các quy tắc định nghĩa trong tệp `.gitlab-ci.yml`.

`rule:if`


```
stages:         # Dictionary
 - build        # this is build stage
 - test         # this is test stage
 - integration  # this is an integration stage
 - prod         # this is prod/production stage

build:              # this is job named build, it can be anything, job1, job2, etc.,
  stage: build      # this job belongs to the build stage. Here both job name and stage name is the same, i.e., build
  script:
    - echo "This is a build step."          # We are running an echo command, but it can be any command.
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'   # this job will get triggered when the branch name is __main__
```

- Công việc build có câu lệnh if sử dụng branch name làm điều kiện trong các quy tắc. Vì vậy, bất kỳ thay đổi nào trong main branch sẽ khởi động các pipeline vì giá trị được so sánh là main. NGoài ra cũng có thể sử dụng bất kỳ biểu thức như (==, !=, =~, ~=) và phép nối/phép tuyển như (&&, ||) rồi kết hợp chúng với sự trợ giúp của các biến được định nghĩa trước từ GitLab để tạo quy trình CI/CD.

- Nếu không có thuộc tính nào được định nghĩa trong thuộc tính rules, các giá trị mặc định sẽ là: `when: on_success` và `allow_failure: false`

`rule:changes`

```
stages:         # Dictionary
 - build        # this is build stage
 - test         # this is test stage
 - integration  # this is an integration stage
 - prod         # this is prod/production stage

build:              # this is job named build, it can be anything, job1, job2, etc.,
  stage: build      # this job belongs to the build stage. Here both job name and stage name is the same, i.e., build
  script:
    - echo "This is a build step."          # We are running an echo command, but it can be any command.
  rules:
    - changes:
      - Dockerfile
```

- Ví dụ trên chỉ rõ công việc có tên build chỉ nên chạy khi tệp có tên Dockerfile bị thay đổi. Vì vậy, nếu chỉnh sửa tệp .gitlab-ci.yml, sau đó sao chép nội dung trên và lưu các thay đổi, pipeline sẽ không được thực thi vì không có thay đổi nào đối với Dockerfile.

`rules:exists`

```
stages:         # Dictionary
 - build        # this is build stage
 - test         # this is test stage
 - integration  # this is an integration stage
 - prod         # this is prod/production stage

build:              # this is job named build, it can be anything, job1, job2, etc.,
  stage: build      # this job belongs to the build stage. Here both job name and stage name is the same, i.e., build
  script:
    - docker build -t $CI_REGISTRY/root/django-nv .
  rules:
    - exists:
      - Dockerfile
```

- Một tác vụ sẽ chạy khi một file nhất định tồn tại. Trong trường hợp trên, quy tắc chỉ định rằng tác vụ sẽ chạy khi file có tên Dockerfile tồn tại, hoặc có thể sử dụng các mẫu glob để khớp với nhiều file trong một directory cụ thể.

`rules:allow_failure`

```
stages:         # Dictionary
 - build        # this is build stage
 - test         # this is test stage
 - integration  # this is an integration stage
 - staging      # this is staging stage
 - prod         # this is prod/production stage

job4:
  stage: staging
  script:
    - echo "This is a deploy step to staging environment."
    - exit 1    # Non zero exit code, fails a job.
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'   # this job will get triggered when the branch name is __main__
      allow_failure: true

job5:
  stage: prod
  script:
    - echo "This is a deploy step to production environment."
  rules:
    - if: '$CI_COMMIT_TAG !~ "/^$/"'   # this job will trigger when you create a new tag
      when: manual
```

