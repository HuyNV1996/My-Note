# Một số lệnh Docker thông dụng và chú thích

## 1. Docker Version và Thông Tin Hệ Thống
- **Hiển thị phiên bản Docker đang sử dụng**
  ```sh
  docker --version
  ```
  ```sh
  docker info
  ```
- **Container**
  ```sh
  docker run -d -p 80:80 nginx
  ```
  ```sh
  docker ps
  ```
  ```sh
  docker ps -a
  ```
  ```sh
  docker stop [CONTAINER_ID]
  ```
  ```sh
  docker rm [CONTAINER_ID]
  ```
- **Image**
  ```sh
  docker images
  ```
  ```sh
  docker pull [IMAGE_NAME]
  ```
  docker rmi [IMAGE_ID]
  ```
- **Logs**
  ```sh
  docker logs [CONTAINER_ID]
  ```
- **Truy cập bên trong container**
  ```sh
  docker exec -it [CONTAINER_ID] /bin/bash
  ```
- **Buil từ docker file**
  ```sh
  docker build -t [IMAGE_NAME] .
  ```
