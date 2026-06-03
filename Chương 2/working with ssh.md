`ssh -i ~/.ssh/id_rsa root@prod-8t8ba53v` - kết nối vào máy bằng private key.

`ssh-keyscan -H prod-8t8ba53v >> ~/.ssh/known_hosts` - thêm fingerprint vào known_hosts

`ssh root@prod-8t8ba53v "hostname"` - chạy lệnh từ xa
