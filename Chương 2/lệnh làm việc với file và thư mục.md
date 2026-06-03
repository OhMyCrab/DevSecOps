Sử dụng ký hiệu & ở cuối câu lệnh sẽ chạy tiến trình ở background (tiến trình ngầm), cho phép thực hiện các tác vụ khác.

`ls`: lệnh liệt kê file và thư mục

`mkdir`: lệnh giúp tạo một thư mục mới

`rmdir`: lệnh xóa thư mục

`pwd`: lệnh hiển thị thư mục làm việc hiện tại

`cd`

`cat`: lệnh đọc file

`nano`: lệnh sửa file

`mv`: lệnh đổi tên file

`rm`: xóa file

Lệnh `cat` chủ yếu được dùng để đọc một tập tin, nhưng bằng cách sử dụng một line magic (hay còn gọi là command line hack), ta có thể tạo một tập tin theo cách không cần tương tác (lý tưởng cho các hệ thống CI/CD)

```
cat > daylatenfile <<EOL

Some text content
Some text content 2
Some text content 3

EOL
```

- `>`: thay vì in ra màn hình, dữ liệu sẽ được ghi vào file `daylatenfile`. Nếu file chưa tồn tại thì sẽ được tạo mới, nếu đã tồn tại thì nội dung cũ sẽ bị ghi đè

- `<<EOL` là cú pháp Here Document, báo cho shell rằng mọi nội dung từ dòng tiếp theo cho đến khi gặp dòng chỉ chứa EOL sẽ được gửi vào standard input của lệnh cat

- Vì `cat` nhận dữ liệu từ stdin và `>` chuyển output của cat vào file, nên toàn bộ nội dung nằm giữa hai dấu EOL sẽ được ghi vào file `daylatenfile`
