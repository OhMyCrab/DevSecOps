SCA

`Safety` - được tích hợp vào GitLab CI/CD bằng python

```
oast:
  stage: test
  script:
    - docker pull hysnsec/safety
    - |
      cat > .safety-policy.yml <<EOF
      security:
        ignore-vulnerabilities: {}
      EOF
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
  artifacts:
    paths: [oast-results.json]
    when: always
  allow_failure: true
```

`RetireJS` - 

```
oast-frontend:
  stage: test
  image: node:alpine3.10
  script:
    - npm install
    - npm install -g retire@5.0.0
    - retire --outputformat json --outputpath retirejs-report.json --severity high
```

SAST

`TruffleHog` - được tích hợp vào GitLab CI/CD bằng Docker

```
git-secrets:
  stage: build
  script:
    - docker run -v $(pwd):/src --rm hysnsec/trufflehog git http://gitlab-ce-8t8ba53v.lab.practical-devsecops.training/root/django-nv.git --fail --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always
    expire_in: one week
  allow_failure: true
```

`Bandit`

```
sast:
  stage: build
  script:
    - docker pull hysnsec/bandit  # tải bandit docker container
    # chạy docker container
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true
```

DAST

`nikto`

```
nikto:
  stage: test
  script:
    - docker pull hysnsec/nikto
    - docker run --rm -v $(pwd):/tmp hysnsec/nikto -h prod-8t8ba53v -o /tmp/nikto-output.xml
  artifacts:
    paths: [nikto-output.xml]
    when: always
  allow_failure: true
```

`sslyze`

```
sslscan:
  stage: test
  script:
    - docker pull hysnsec/sslyze
    - docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-8t8ba53v.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json
  artifacts:
    paths: [sslyze-output.json]
    when: always
  allow_failure: true
```

`nmap`

```
nmap:
  stage: test
  script:
    - docker pull hysnsec/nmap
    - docker run --rm -v $(pwd):/tmp hysnsec/nmap prod-8t8ba53v -oX /tmp/nmap-output.xml
  artifacts:
    paths: [nmap-output.xml]
    when: always
  allow_failure: true
```

`zap`

```
zap-baseline:
  stage: integration
  before_script:
    - docker pull hysnsec/zap:2.16.1
  script:
    - docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm hysnsec/zap:2.16.1 zap-baseline.py -t https://prod-8t8ba53v.lab.practical-devsecops.training -J zap-output.json
  after_script:
    - docker rmi hysnsec/zap:2.16.1
  artifacts:
    paths: [zap-output.json]
    when: always        # What does this do?
  allow_failure: true  # Optional
```

** Ví dụ về challenge 1 trong thi thử

```
stages:
  - build
  - test
  - artefact_scanning
  - preprod
  - integration
  - release
  - prod

build:
  stage: build
  script:
    - echo "This is a build step"
    - echo "some tool output" > output.txt
  artifacts:
    paths: [output.txt]

trufflehog:
  stage: build
  script:
    - docker run --rm -v $(pwd):/src hysnsec/trufflehog git . --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always
    expire_in: 1 week
  allow_failure: true

bandit:
  stage: build
  script:
    - docker run --rm --user $(id -u):$(id -g) -v $(pwd):/src hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true

safety:
  stage: test
  script:
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > safety-output.json
  artifacts:
    paths: [safety-output.json]
    when: always
  allow_failure: true

retirejs:
  stage: test
  image: node:alpine3.10
  script:
    - npm install -g retire@5.0.0
    - retire --outputformat json --outputpath retirejs-output.json --severity high
  artifacts:
    paths: [retirejs-output.json]
    when: always
  allow_failure: true

integration:
  stage: integration
  script:
    - echo "This is an integration step"
    - exit 1
  allow_failure: true

nmap:
  stage: integration
  script:
    - docker run --rm -v $(pwd):/tmp hysnsec/nmap $PROD_URL -oX /tmp/nmap-output.xml
  artifacts:
    paths: [nmap-output.xml]
    when: always
  allow_failure: true

nikto:
  stage: integration
  script:
    - docker run --rm -v $(pwd):/tmp hysnsec/nikto -h $PROD_URL -o /tmp/nikto-output.xml
  artifacts:
    paths: [nikto-output.xml]
    when: always
  allow_failure: true

sslyze:
  stage: integration
  script:
    - docker run --rm -v $(pwd):/tmp hysnsec/sslyze $PROD_URL:443 --json_out /tmp/sslyze-output.json
  artifacts:
    paths: [sslyze-output.json]
    when: always
  allow_failure: true

zap:
  stage: integration
  script:
    - docker run --rm --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw hysnsec/zap:2.16.1 zap-baseline.py -t https://$PROD_URL -J zap-output.json
  artifacts:
    paths: [zap-output.json]
    when: always
  allow_failure: true

prod:
  stage: prod
  script:
    - echo "This is a deploy step"
  when: manual
```

CINC auditor

```
stages:
  - build
  - test
  - artefact_scanning
  - preprod
  - integration
  - release
  - prod

build:
  stage: build
  script:
    - echo "This is a build step"
    - echo "some tool output" > output.txt
  artifacts:
    paths: [output.txt]

trufflehog:
  stage: build
  script:
    - docker run --rm -v $(pwd):/src hysnsec/trufflehog git . --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always
    expire_in: 1 week
  allow_failure: true

bandit:
  stage: build
  script:
    - docker run --rm --user $(id -u):$(id -g) -v $(pwd):/src hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true

safety:
  stage: test
  script:
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > safety-output.json
  artifacts:
    paths: [safety-output.json]
    when: always
  allow_failure: true

retirejs:
  stage: test
  image: node:alpine3.10
  script:
    - npm install -g retire@5.0.0
    - retire --outputformat json --outputpath retirejs-output.json --severity high
  artifacts:
    paths: [retirejs-output.json]
    when: always
  allow_failure: true

cinc:
  stage: preprod
  image: ubuntu:22.04

  before_script:
    - apt-get update && apt-get install -y curl openssh-client
    - curl https://omnitruck.cinc.sh/install.sh | bash -s -- -P cinc-auditor -v 6
    - mkdir -p ~/.ssh
    - echo "$PROD_SSH_PRIVKEY" > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config

  script:
    - cinc-auditor exec https://github.com/dev-sec/linux-baseline.git -t ssh://root@$PROD_URL -i ~/.ssh/id_rsa --chef-license accept --reporter json > cinc-results.json
  artifacts:
    paths: [cinc-results.json]
    when: always
  allow_failure: true

integration:
  stage: integration
  script:
    - echo "This is an integration step"
    - exit 1
  allow_failure: true

nmap:
  stage: integration
  script:
    - docker run --rm -v $(pwd):/tmp hysnsec/nmap $PROD_URL -oX /tmp/nmap-output.xml
  artifacts:
    paths: [nmap-output.xml]
    when: always
  allow_failure: true

nikto:
  stage: integration
  script:
    - docker run --rm -v $(pwd):/tmp hysnsec/nikto -h $PROD_URL -o /tmp/nikto-output.xml
  artifacts:
    paths: [nikto-output.xml]
    when: always
  allow_failure: true

sslyze:
  stage: integration
  script:
    - docker run --rm -v $(pwd):/tmp hysnsec/sslyze $PROD_URL:443 --json_out /tmp/sslyze-output.json
  artifacts:
    paths: [sslyze-output.json]
    when: always
  allow_failure: true

zap:
  stage: integration
  script:
    - docker run --rm --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw hysnsec/zap:2.16.1 zap-baseline.py -t https://$PROD_URL -J zap-output.json
  artifacts:
    paths: [zap-output.json]
    when: always
  allow_failure: true

prod:
  stage: prod
  script:
    - echo "This is a deploy step"
  when: manual
```
