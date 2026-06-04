Bandit tool được thiết kế để tìm ra các vấn đề bảo mật phổ biến trong Python code.

`pip3 install bandit==1.8.5`: lệnh tải bandit tool

`bandit -r . -f json | tee bandit-output.json`

- `-r .`: quét toàn bộ thư mục hiện tại

- `-f json`: xuất kết quả dưới dạng JSON

`bandit -r . -l`: lệnh lọc low-level vul

`bandit -r . -ll`: lệnh lọc medium-level vul

`bandit -r . -lll`: lệnh lọc high-level vul

