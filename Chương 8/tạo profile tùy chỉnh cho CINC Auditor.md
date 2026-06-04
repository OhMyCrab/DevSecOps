`curl https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -P cinc-auditor -v 6`

`mkdir cinc-profiles && cd cinc-profiles`

`cinc-auditor init profile ubuntu --chef-license accept`

`cinc-auditor check ubuntu`

`cinc-auditor exec ubuntu`

`echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config`

`cinc-auditor exec ubuntu -t ssh://root@prod-8t8ba53v -i ~/.ssh/id_rsa --chef-license accept`

