** False Positive

Có hai cách để nhận biết cảnh báo giả: đọc mã nguồn hoặc khai thác lỗ hổng

Những điểu quan trọng để nhận biết cảnh báo giả: 

- Đọc mã nguồn thực tế - Đừng chỉ dựa vào khả năng phát hiện của trình quét.

- Hiểu secure coding patterns - Truy vấn tham số so với nối chuỗi.

- Kiểm tra ngữ cảnh - Đoạn flagged code được sử dụng như thế nào trong thực tế

- Kiểm tra luồng dữ liệu - Dữ liệu đến từ đâu và được xử lý như nào trong thực tế

- Hiểu rõ tool - Nắm vững những hạn chế và logic phát hiện của tool.

** Dùng tính năng baseline feature của bandit để lọc cảnh báo giả

baseline.json là file dùng để lưu trữ/đánh dấu các cảnh báo giả

Khi file baseline.json có dữ liệu, bandit sẽ không báo cáo các vấn đề có trong file baseline.json trong các lần quét tiếp theo

Ví dụ:

```
cat > baseline.json<<EOF
{
  "results": [
    {
      "code": "12 username = 'admin'\n13 password = 'secret'\n14 \n15 # Disqus Configuration\n16 disqus_shortname = 'blogpythonlearning'  # please change this.\n",
      "col_offset": 11,
      "filename": "./flaskblog/config.py",
      "issue_confidence": "MEDIUM",
      "issue_cwe": {
        "id": 259,
        "link": "https://cwe.mitre.org/data/definitions/259.html"
      },
      "issue_severity": "LOW",
      "issue_text": "Possible hardcoded password: 'secret'",
      "line_number": 13,
      "line_range": [
        13,
        14,
        15
      ],
      "more_info": "https://bandit.readthedocs.io/en/1.7.4/plugins/b105_hardcoded_password_string.html",
      "test_id": "B105",
      "test_name": "hardcoded_password_string"
    }
  ]
}
EOF
```

<img width="619" height="222" alt="image" src="https://github.com/user-attachments/assets/89e975a7-c2f6-4c0a-9262-e1efe80da1c6" />

`bandit -r . -f json -b baseline.json`

`bandit -r . -f json -b baseline.json | jq '.results | length'`

Khi dùng baseline feature đã lọc cảnh báo giả, do đó từ 15 cảnh báo -> 14

