`docker pull`: có thể ẩn kết quả đầu ra bằng options --quiet hoặc -q

- `docker pull ubuntu` - tải (pull) image ubuntu từ Docker Hub về máy local.

`docker run`: tạo + chạy container

- `docker run -d -p 5000:5000 --restart=always --name registry registry:2`

`docker exec`: chạy lệnh trong container đang chạy

`docker ps`: hiển thị container đang chạy

- `docker ps -a` - hiển thị tất cả container

`docker top`: xem process trong container

`docker rm`: xóa container

- `docker rm myubuntu`

`docker inspect`: lệnh dùng để hiển thị thông tin chi tiết về một đối tượng Docker, chẳng hạn như container, image hoặc network.

`docker logs`: lệnh xem log/output của container.

- `docker logs custom-nginx-container`

`docker stop`: lệnh dừng container

** Build Docker Image
  
- `cat Dockerfile` - Dockerfile là file chứa các chỉ thị để Docker build image.

`docker build`: lệnh build docker image

- `docker build -t django.nv:1.0 .` - `-t`: tag image; `django.nv`: tên image; `1.0`: version/tag; `.` thư mục hiện tại chứa Dockerfile

`docker images`: lệnh xem images

** Quản lý dữ liệu trong Docker

`docker volume`

- `docker volume ls` - liệt kê các volume

- `docker volume create demo` - tạo volume `demo`

- `docker volume inspect demo` - xem chi tiết volume `demo`

- `docker volume rm demo` - xóa volume `demo`

- `docker volume prune` - xóa toàn bộ volume không sử dụng

- `docker run --name ubuntu -d -v demo:/opt -it ubuntu:24.04` - `-v demo:/opt`: Docker Volume. Dữ liệu vẫn tồn tại sau khi xóa container.

-  `docker run --name ubuntu2 -d -v /opt:/opt -it ubuntu:24.04` - `-v /opt:/opt`: Bind Mount - Host quản lý dữ liệu

** Kho lưu trữ Docker

`docker tag`: lệnh gắn tag để chuẩn bị push image lên Docker Registry

** Docker Networking

`docker network`

- `docker network ls` - liệt kê available networks

- `docker network list` - liệt kê available networks

- `docker network create mynetwork` - tạo network mới có tên `mynetwork`

- `docker network rm mynetwork` -  xóa một network có tên `mynetwork`.

** Dockerfile

```
FROM        : image nền
RUN         : chạy lệnh khi build image
COPY        : copy file từ host vào image
ADD         : copy file vào image, hỗ trợ URL và tự giải nén archive.
CMD         : lệnh mặc định khi container start
ENTRYPOINT  : chương trình chính của container
```

Container sống khi PID 1 còn chạy.

RUN chạy lúc build.

CMD/ENTRYPOINT chạy lúc container start.

Nếu có nhiều CMD => chỉ CMD cuối cùng có hiệu lực.
