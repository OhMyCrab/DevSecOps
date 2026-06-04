Settings → CI/CD → Variables → Expand

<img width="1710" height="711" alt="image" src="https://github.com/user-attachments/assets/7329841e-8109-474d-9a3e-3ff43505e8a3" />

<img width="388" height="465" alt="image" src="https://github.com/user-attachments/assets/c3870b19-1b0b-4b1b-81c0-3cde1fe584d6" />

<img width="533" height="514" alt="image" src="https://github.com/user-attachments/assets/83a0f19a-abd5-41aa-b95d-48d839ed374b" />

`ssh root@prod-8t8ba53v`: vào máy ảo

`more /root/.ssh/id_rsa`: private key của máy ảo

Sau khi các biến được lưu trên hệ thống CI thì cần chỉnh sửa lại nội dung file .gitlab-ci.yml và thay thế prod job bằng đoạn mã sau:

```
prod:
  stage: deploy
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
        docker login -u ${CI_REGISTRY_USER} -p ${CI_REGISTRY_PASSWORD} ${CI_REGISTRY}
        docker rm -f django.nv
        docker run -d --name django.nv -p 8000:8000 $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
      EOF
```

