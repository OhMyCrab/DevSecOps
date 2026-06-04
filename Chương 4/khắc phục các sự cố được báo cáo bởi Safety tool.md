Quy trình xử lý dependency vulnerability trong DevSecOps

```
Safety Scan
     ↓
Phát hiện package có CVE
     ↓
Xác định package lỗi
(requirements.txt)
     ↓
Tìm phiên bản đã vá
     ↓
Nâng cấp package
     ↓
Quét lại bằng Safety
     ↓
Xác nhận hết CVE
     ↓
Chạy test để đảm bảo ứng dụng không bị hỏng
```

Safety phát hiện dependency có lỗ hổng → nâng cấp dependency lên phiên bản đã vá → quét lại xác nhận hết CVE → kiểm thử ứng dụng để đảm bảo không bị ảnh hưởng.
