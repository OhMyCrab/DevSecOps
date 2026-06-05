```
services:
  - docker:dind

stages:
  - build
  - deploy
  - test

build:
  stage: build
  before_script:
   - echo $CI_REGISTRY_PASSWORD | docker login -u $CI_REGISTRY_USER --password-stdin $CI_REGISTRY
  script:
   - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
   - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

prod:
  stage: deploy
  image: kroniak/ssh-client:3.6
  environment: production
  needs:
    - build
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
        docker run -d --name django.nv -p 8000:8000 $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
      EOF
```
