`ssh -i ~/.ssh/id_rsa root@prod-xxx` - kết nối vào máy bằng private key.

- -i cho phép chúng ta chỉ định khóa riêng tư để đăng nhập vào máy từ xa

- root là người dùng mà chúng ta muốn đăng nhập

- prod-xxx là tên máy chủ

`ssh-keyscan -H prod-xxx >> ~/.ssh/known_hosts` - thêm fingerprint vào known_hosts

`ssh root@prod-xxx "hostname"` - chạy lệnh từ xa trên máy prod-xxx
