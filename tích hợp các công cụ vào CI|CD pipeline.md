SCA

`Safety`

test local

```
git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp

cd webapp

apt-get update && apt-get install -y build-essential python3-dev # nếu tải safety lỗi thì dùng

pip3 install safety==2.3.5 # lệnh tải Safety

cat > .safety-policy.yml <<EOF
security:
  ignore-vulnerabilities: {}
EOF

safety check -r requirements.txt --json | tee safety-output.json # lệnh chạy Safety
```

tích hợp vào pipeline

```
safety:
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

test local

```
git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp

cd webapp

# tải Node JS và NPM
mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
apt update

apt install nodejs -y

npm install -g retire@5.0.0 # lệnh tải RetireJS

retire --severity critical --outputformat json --outputpath retire_output.json # lệnh chạy RetireJS
```

tích hợp vào pipeline

```
retirejs:
  stage: test
  image: node:alpine3.10
  script:
    - npm install
    - npm install -g retire@5.0.0
    - retire --outputformat json --outputpath retirejs-report.json --severity high
```

SAST

`TruffleHog`

test local

```
# tải TruffleHog
wget https://github.com/trufflesecurity/trufflehog/releases/download/v3.79.0/trufflehog_3.79.0_linux_amd64.tar.gz
tar -xvf trufflehog_3.79.0_linux_amd64.tar.gz
chmod +x trufflehog
mv trufflehog /usr/local/bin/

trufflehog git http://gitlab-ce-8t8ba53v.lab.practical-devsecops.training/root/django-nv.git --json # lệnh chạy TruffleHog quét dự án django.nv
```

```
eval "$(ssh-agent -s)"
chmod 600 ~/.ssh/id_rsa

ssh-add ~/.ssh/id_rsa

cat << EOF > ~/.ssh/config
Host gitlab-ce-8t8ba53v
    HostName gitlab-ce-8t8ba53v
    User git
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes
EOF

trufflehog git git@gitlab-ce-8t8ba53v:root/django-nv.git --json | tee secret.json # lệnh chạy TruffleHog dùng ssh để quét git repo
```

tích hợp vào pipeline

```
trufflehog:
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

test local

```
git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp

cd webapp

pip3 install bandit==1.8.5 # lệnh tải bandit

bandit -r . -f json | tee bandit-output.json # lệnh chạy bandit
```

tích hợp vào pipeline

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

test local

```
apt install -y libnet-ssleay-perl # lệnh cần thiết để hỗ trợ quét SSL trong Nikto.

git clone https://github.com/sullo/nikto # lệnh tải nikto

cd nikto/program
git checkout tags/2.1.6

./nikto.pl -h prod-8t8ba53v -output nikto_output.xml # lệnh chạy nikto
```

tích hợp vào pipeline

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

test local

```
pip3 install sslyze==6.0.0 # lệnh tải sslyze

sslyze --json_out sslyze-output.json prod-8t8ba53v.lab.practical-devsecops.training:443 # lệnh chạy sslyze
```

tích hợp vào pipeline

```
sslyze:
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

test local

```
apt-get update && apt-get install nmap -y  # lệnh tải nmap

nmap prod-8t8ba53v -oX nmap_out.xml # lệnh chạy nmap
```

tích hợp vào pipeline

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

test local

```
docker run --rm hysnsec/zap:2.16.1 zap-baseline.py -t https://prod-8t8ba53v.lab.practical-devsecops.training # lệnh tải zap

docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm hysnsec/zap:2.16.1 zap-baseline.py -t https://prod-8t8ba53v.lab.practical-devsecops.training -J zap-output.json # lệnh chạy zap
```

tích hợp vào pipeline

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

IaC

`ansible`

test local

```
pip3 install ansible==9.13.0

cat > inventory.ini <<EOL

# DevSecOps Studio Inventory
[devsecops]
devsecops-box-8t8ba53v

[gitservers]
gitlab-ce-8t8ba53v

[prod]
prod-8t8ba53v
EOL

ssh-keyscan -H prod-8t8ba53v gitlab-ce-8t8ba53v devsecops-box-8t8ba53v >> ~/.ssh/known_hosts

ansible-galaxy install dev-sec.os-hardening

cat > ansible-hardening.yml <<EOL
---
- name: Playbook to harden Ubuntu OS.
  hosts: prod
  remote_user: root
  become: yes

  vars:
    # Exclude sysctl parameters that cannot be changed at runtime in newer Ubuntu
    # The role will skip these parameters when they're in the ignore list
    os_hardening_sysctl_ignore:
      - kernel.randomize_va_space
      - kernel.core_uses_pid
      - vm.mmap_rnd_bits
      - vm.mmap_rnd_compat_bits
      - kernel.kexec_load_disabled
      - fs.suid_dumpable

  roles:
    - role: dev-sec.os-hardening
      # Allow role to continue even if some sysctl parameters fail
      # (they are read-only in newer Ubuntu versions)
      ignore_errors: yes

EOL

ansible-playbook -i inventory.ini ansible-hardening.yml

```

tích hợp vào pipeline

```
ansible-hardening:
  stage: prod
  image: willhallonline/ansible:2.16-ubuntu-22.04
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -H $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - echo -e "[prod]\n$DEPLOYMENT_SERVER" >> inventory.ini
    - ansible-galaxy install dev-sec.os-hardening
    - ansible-playbook -i inventory.ini ansible-hardening.yml
```

```
---
- name: Playbook to harden Ubuntu OS.
  hosts: prod
  remote_user: root
  become: yes

  vars:
    # Exclude sysctl parameters that cannot be changed at runtime in newer Ubuntu
    # The role will skip these parameters when they're in the ignore list
    os_hardening_sysctl_ignore:
      - kernel.randomize_va_space
      - kernel.core_uses_pid
      - vm.mmap_rnd_bits
      - vm.mmap_rnd_compat_bits
      - kernel.kexec_load_disabled
      - fs.suid_dumpable

  roles:
    - role: dev-sec.os-hardening
      # Allow role to continue even if some sysctl parameters fail
      # (they are read-only in newer Ubuntu versions)
      ignore_errors: yes
```

CaC

`CinC audit`

test local

```
wget https://omnitruck.cinc.sh/install.sh

bash install.sh -P cinc-auditor -v 6

echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config

cinc-auditor exec https://github.com/dev-sec/linux-baseline.git -t ssh://root@prod-8t8ba53v -i ~/.ssh/id_rsa --chef-license accept
```

tích hợp vào pipeline

```
cinc-auditor:
  stage: prod
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -H $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share cincproject/auditor:6.8.24 exec https://github.com/dev-sec/linux-baseline.git -t ssh://root@$DEPLOYMENT_SERVER -i ~/.ssh/id_rsa --chef-license accept
```

tạo CinC audit profile tùy chỉnh

```
mkdir cinc-profiles && cd cinc-profiles

cinc-auditor init profile ubuntu --chef-license accept

cat > ubuntu/controls/example.rb <<EOL
control 'shadow-1' do
  title 'Ensure /etc/shadow file is properly secured'
  desc 'The /etc/shadow file contains password hashes and should be protected'

  describe file('/etc/shadow') do
    it { should exist }
    it { should be_file }
    it { should be_owned_by 'root' }
  end
end
EOL

cinc-auditor check ubuntu

cinc-auditor exec ubuntu

echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config

cinc-auditor exec ubuntu -t ssh://root@prod-8t8ba53v -i ~/.ssh/id_rsa --chef-license accept
```

Vul Management

`defect dojo`

test local

```
git clone https://gitlab.practical-devsecops.training/pdso/rails.git webapp

cd webapp

docker run --rm -v $(pwd):/src hysnsec/brakeman -f json /src

docker run --rm -v $(pwd):/src hysnsec/brakeman -f json /src | tee brakeman-result.json

curl https://gitlab.practical-devsecops.training/-/snippets/28/raw -o upload-results.py

pip3 install requests

export API_KEY=$(curl -s -XPOST -H 'content-type: application/json' https://dojo-8t8ba53v.lab.practical-devsecops.training/api/v2/api-token-auth/ -d '{"username": "root", "password": "pdso-training"}' | jq -r '.token' )

python3 upload-results.py --host dojo-8t8ba53v.lab.practical-devsecops.training --api_key $API_KEY --engagement_id 2 --product_id 3 --lead_id 1 --environment "Production" --result_file brakeman-result.json --scanner "Brakeman Scan"

bandit -r . -f json | tee bandit-output.json
```

tích hợp vào pipeline

```
curl https://gitlab.practical-devsecops.training/-/snippets/28/raw -o upload-results.py
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
    - docker run --rm -v $(pwd):/src hysnsec/trufflehog filesystem /src --json | tee trufflehog-output.json
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

test:
  stage: test
  script:
    - echo "This is a test step"

cinc:
  stage: preprod
  image: ubuntu:22.04

  before_script:
    - apt-get update && apt-get install -y git
    - apt-get update && apt-get install -y curl openssh-client netcat-openbsd dnsutils
    - curl -s https://omnitruck.cinc.sh/install.sh | bash -s -- -P cinc-auditor -v 6
    - mkdir -p ~/.ssh
    - echo "$PROD_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - ssh-keyscan -H $PROD_HOSTNAME >> ~/.ssh/known_hosts
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - echo "StrictHostKeyChecking no" >> ~/.ssh/config 

  script:
    - cinc-auditor exec https://github.com/dev-sec/linux-baseline.git -t ssh://$PROD_USERNAME@$PROD_HOSTNAME -i ~/.ssh/id_rsa --chef-license accept --reporter json > cinc-results.json

  artifacts:
    when: always
    paths:
      - cinc-results.json

  allow_failure: true

integration:
  stage: integration
  script:
    - echo "This is an integration step"
    - exit 1
  allow_failure: true

prod:
  stage: prod
  script:
    - echo "This is a deploy step"
  when: manual
```
