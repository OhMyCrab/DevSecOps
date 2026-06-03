`cat /etc/passwd > mypasswd.txt`

`cat /etc/passwd >> mypasswd.txt` -  `>>` là toán tử nối thêm, dùng để thêm kết quả vào cuối tệp được chỉ định (mypasswd.txt) thay vì ghi đè lên tệp đó.

`echo`

`cat mypasswd.txt | cut -d ':' -f 1` - lệnh tách mỗi dòng thành nhiều trường dựa trên dấu `:`, lấy trường số 1 (phần nằm trước dấu `:` đầu tiên) và hiển thị ra màn hình.

`ps -aux | grep bash` - hiển thị danh sách các tiến trình đang chạy
