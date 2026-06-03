`stat`: lệnh hiển thị thông tin chi tiết về một tệp, bao gồm kích thước, quyền truy cập, quyền sở hữu và dấu thời gian.

`chmod`: lệnh cấp quyền

`chmod +x myfile` - cấp quyền thực thi cho file `myfile`

`./myfile` - thực thi file `myfile`, dấu chấm (./) ở đầu lệnh cho hệ điều hành Linux biết rằng tệp thực thi này nằm trong thư mục hiện tại.

`sudo ls`

`echo -e "pdevsecops\npdevsecops" | adduser --gecos "" john` - tạo người dùng có tên john

`usermod -aG sudo john` - sử dụng lệnh usermod để thêm người dùng john vào nhóm sudo.

`sudo su - john` - lệnh trở thành john

`echo pdevsecops | sudo -S cat /etc/shadow` - sudo cho phép thực thi lệnh với quyền cao hơn nếu được cấp phép, /etc/shadow là file nhạy cảm chứa thông tin mật khẩu đã băm (hash), nên mặc định user thường không được đọc.
