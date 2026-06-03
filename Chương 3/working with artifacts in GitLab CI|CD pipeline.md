Artifact là kết quả trung gian hoặc cuối cùng (binary, file log, report, docker image...) của các giai đoạn build, test.

Artifact đóng vai trò quan trọng trong quy trình CI/CD vì nó:

- Thể hiện trạng thái của một dự án tại một thời điểm cụ thể.

- Có thể được lưu giữ và chia sẻ giữa các giai đoạn khác nhau của quy trình.

- Có thể được sử dụng để khôi phục bản phát hành về phiên bản trước đó nếu có sự cố xảy ra.

Sơ đồ quy trình minh họa cách thức hoạt động của pipeline với các artifact.

<img width="1024" height="576" alt="image" src="https://github.com/user-attachments/assets/26956b5e-a73f-4f7d-8ebb-5e338144216d" />

- Đầu tiên, user kích hoạt pipeline trong CI/CD application bằng cách sử dụng pipeline file (.gitlab-ci.yml, Jenkinsfile, v.v.). Sau khi xác định một số công việc, pipeline sẽ chạy từng công việc theo trình tự dựa trên các quy tắc đã thiết lập.

- Tiếp theo, công việc được chạy trong pipeline process. Khi pipeline file bao gồm quy tắc lưu trữ output dưới dạng artifacts, công việc sẽ tạo ra artifact sau khi hoàn thành thành công.

- Cuối cùng, sau khi tất cả các quy trình trong pipeline processes hoàn tất, deployment process sẽ được kích hoạt.

** `docker run -i -v $(pwd):/app alpine sh -c "echo '{\"vulnerability\":\"XSS Injection\"}' > /app/vulnerabilities.json"` - tạo ra một Artifact (file vulnerabilities.json) thông qua Docker container.

- `docker run`: Khởi chạy một container từ image alpine.

- `-i`: Chế độ tương tác (interactive).

- `-v $(pwd)`:/app: Đây là phần quan trọng nhất—Volume Mount. Nó ánh xạ thư mục hiện tại của máy host (nơi bạn chạy lệnh) vào thư mục /app bên trong container.

- `alpine sh -c "..."`: Chạy lệnh shell bên trong container để tạo file.

Kết quả: File vulnerabilities.json được tạo ra bên trong container nhưng do cơ chế Mount, nó sẽ xuất hiện trực tiếp tại thư mục hiện tại trên máy của bạn.
