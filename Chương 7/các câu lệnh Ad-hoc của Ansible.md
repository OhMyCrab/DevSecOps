file inventory là một file dùng để định nghĩa danh sách các máy chủ có thể được sắp xếp thành các nhóm, nó cung cấp khả năng lưu trữ và quản lý một số biến.

```
cat > inventory.ini <<EOL

[devsecops]
devsecops-box-8t8ba53v

[sandbox]
sandbox-8t8ba53v

[prod]
prod-8t8ba53v

EOL
```

`ansible -i inventory.ini prod --list-hosts`

`ansible -i inventory.ini gitlab --list-hosts`

`ansible --version`

Ansible sử dụng lệnh ad-hoc để thực thi một tác vụ duy nhất trên một hoặc nhiều máy chủ từ xa, và cách thực thi lệnh này dễ dàng và nhanh chóng, nhưng nó không thể tái sử dụng như playbook.

`ansible -i inventory.ini all -m ping`

`ansible -i inventory.ini all -m shell -a "hostname"`

`ansible -i inventory.ini all -m apt -a "name=ntp"`

`ansible-doc -l | egrep "add_host|amazon.aws.aws"`

