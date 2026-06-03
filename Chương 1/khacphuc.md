* Lưu ý: Kết nối SSH chỉ được cho phép đến một vài máy cụ thể. Ví dụ: Gitlab, Gitlab runner. GitLab CE không cho phép kết nối SSH để tránh những thay đổi không mong muốn.

* Nếu máy ảo không hoạt động có thể thử kết nối SSH vào đó.

`ssh prod-8t8ba53v`

Nếu truy cập được URL, điều đó có nghĩa là máy ảo đang hoạt động tốt

Nếu thấy lỗi 503 Service Available, điều đó có nghĩa là dịch vụ web không hoạt động.

* Nếu dịch vụ web không hoạt động, có thể fix như sau:

Đầu tiên, hãy kết nối SSH vào máy ảo

`ssh prod-8t8ba53v`

Sau đó, hãy thử khởi động lại máy chủ web Nginx.

`systemctl start nginx`
