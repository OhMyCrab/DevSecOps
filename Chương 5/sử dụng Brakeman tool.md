Brakeman là một công cụ dành cho các ứng dụng Rails để tìm kiếm các lỗ hổng bảo mật. Nó nhanh, linh hoạt và có khả năng báo cáo rất tốt, do đó rất phù hợp để tích hợp vào quy trình CI/CD.

Cần cài đặt Ruby, vì vậy hãy cập nhật apt.

`apt update`

`apt install ruby-full -y`: lệnh cài ruby

`gem install brakeman -v 5.2.1`: lệnh cài brakeman tool

Lệnh quét của brakeman tool, hiển thị và lưu vào result.json

`brakeman -f json | tee result.json`

Có khá nhiều vấn đề, có thể bỏ qua các vấn đề bằng cách sử dụng file brakeman.ignore.

```
cat > brakeman.ignore <<EOF
{
    "ignored_warnings": [
        {
          "fingerprint": "febb21e45b226bb6bcdc23031091394a3ed80c76357f66b1f348844a7626f4df",
          "note": "ignore XSS"
        }
    ]
}
EOF
```
