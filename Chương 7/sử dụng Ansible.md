Ansible sử dụng ngôn ngữ đơn giản, giống tiếng Anh, để tự động hóa cấu hình, cài đặt và triển khai trong môi trường truyền thống và điện toán đám mây. Nó dễ học và ngay cả những người không chuyên về kỹ thuật cũng có thể hiểu được.

`pip3 install ansible==9.13.0 ansible-lint==6.8.1`

Tạo tệp inventory hoặc CMDB cho Ansible

```
cat > inventory.ini <<EOL

# DevSecOps Studio Inventory
[devsecops]
devsecops-box-8t8ba53v

[prod]
prod-8t8ba53v

EOL
```

`ssh-keyscan -H prod-8t8ba53v >> ~/.ssh/known_hosts`

Chạy lệnh ad-hoc của Ansible để cài đặt dịch vụ NTP và kiểm tra phiên bản Bash của tất cả các hệ thống.

`ansible -i inventory.ini  prod -m apt -a "name=ntp state=present"`

 Tìm phiên bản bash được cài đặt trên tất cả các máy trong các inventory file.

 `ansible -i inventory.ini all -m command -a "bash --version"`
