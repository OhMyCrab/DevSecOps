Nikto là một công cụ đánh giá bảo mật máy chủ web. Nó được thiết kế để tìm kiếm các tệp mặc định, tệp không an toàn, các cấu hình sai hoặc không bảo mật, cũng như các chương trình tiềm ẩn rủi ro trên nhiều loại máy chủ web khác nhau.

Nikto được xây dựng dựa trên thư viện LibWhisker2 (do RFP phát triển) và có thể chạy trên bất kỳ nền tảng nào có môi trường Perl.

Cài đặt Nikto

`apt install -y libnet-ssleay-perl` - thiết lập môi trường perl

- lệnh trên cần thiết để hỗ trợ quét SSL trong Nikto.

`git clone https://github.com/sullo/nikto`

`cd nikto/program
git checkout tags/2.1.6`

`./nikto.pl -output nikto_output.xml -h prod-8t8ba53v`

- `-h`: dùng để thiết lập mục tiêu muốn quét

- `-output`: dùng để thiết lập output file mà chúng ta muốn lưu kết quả

`cat nikto_output.xml`

- `SKIPIDS`:	bỏ qua các bài kiểm tra (test ID) dễ gây false positive.

- `SKIPPORTS`:	bỏ qua các cổng (port) không muốn quét, dù chúng đang mở.

- `CLIOPTS`:	thiết lập các tùy chọn dòng lệnh cho Nikto (target, output, scan options...).

- `STATIC-COOKIE`:	gửi một giá trị Cookie cố định trong mọi request HTTP.

- `TIMEOUT`:	đặt thời gian chờ (giây) cho mỗi request HTTP.

- `EXTRAREQS`:	bật/tắt các request HTTP bổ sung để tìm thêm thông tin hoặc lỗ hổng (0 = tắt, 1 = bật).
