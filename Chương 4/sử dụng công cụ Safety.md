Safety kiểm tra các thư viện phụ thuộc đã cài đặt để tìm các lỗ hổng bảo mật đã biết. Theo mặc định, nó sử dụng cơ sở dữ liệu lỗ hổng Python mở Safety DB nhưng có thể được nâng cấp để sử dụng API Safety của pyup.io bằng tùy chọn –key.

`pip3 install safety==2.3.5`: lệnh tải safety tool

Tạo tệp .safety-policy.yml trong cùng thư mục với tệp .requirements.txt:

```
cat > .safety-policy.yml <<EOF
security:
  ignore-vulnerabilities: {}
EOF
```

`security`: phần cấp cao nhất dành cho các quy tắc quét và báo cáo 

`ignore-vulnerabilities`: map của vulnerability IDs cần bỏ qua; map rỗng {} có nghĩa là không có gì bị bỏ qua

`safety check -r requirements.txt --json | tee safety-output.json`

- Sử dụng lệnh `tee` để hiển thị kết quả đầu ra và lưu trữ nó vào một tệp tin cùng một lúc.

- Sử dụng `-r` để chỉ định input file.

- `-json` cho biết đầu ra phải ở định dạng JSON.

