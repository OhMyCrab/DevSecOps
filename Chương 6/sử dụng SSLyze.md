SSLyze là công cụ dùng để đánh giá mức độ an toàn của cấu hình SSL/TLS trên máy chủ, giúp phát hiện chứng chỉ lỗi, thuật toán mã hóa yếu và các lỗ hổng liên quan đến SSL/TLS.

`pip3 install sslyze==6.0.0`: lệnh tải sslyze

`sslyze --json_out sslyze-output.json prod-8t8ba53v.lab.practical-devsecops.training:443`

- `--json_out` được sử dụng để lưu trữ kết quả đầu ra ở định dạng JSON.

cat sslyze-output.json
