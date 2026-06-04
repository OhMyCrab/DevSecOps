Trufflehog giúp tìm kiếm các thông tin secret được lưu trữ trong Git repository.

Tải Trufflehog

```
wget https://github.com/trufflesecurity/trufflehog/releases/download/v3.79.0/trufflehog_3.79.0_linux_amd64.tar.gz
tar -xvf trufflehog_3.79.0_linux_amd64.tar.gz
chmod +x trufflehog
mv trufflehog /usr/local/bin/
```

Ví dụ scan: `trufflehog git http://gitlab-ce-8t8ba53v.lab.practical-devsecops.training/root/django-nv.git --json`

<img width="992" height="547" alt="image" src="https://github.com/user-attachments/assets/df310c3f-a718-4664-bda7-5946ba640425" />

Ta có thể tiến hành quét kho lưu trữ Git bằng SSH, sau đó chuyển đổi kết quả thành tệp secret.json.

Trước tiên cần thiết lập ssh agent để xác thực với GitLab repository.

```
eval "$(ssh-agent -s)"
chmod 600 ~/.ssh/id_rsa
```
Thêm private key của mình vào ssh agent.

`ssh-add ~/.ssh/id_rsa`

Sau đó thêm cấu hình SSH vào tệp `~/.ssh/config`.

```
cat << EOF > ~/.ssh/config
Host gitlab-ce-8t8ba53v
    HostName gitlab-ce-8t8ba53v
    User git
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes
EOF
```

`trufflehog git git@gitlab-ce-8t8ba53v:root/django-nv.git --json | tee secret.json`

- Sử dụng lệnh tee để hiển thị kết quả đầu ra và lưu trữ nó vào file cùng lúc.

`cat secret.json | jq`
