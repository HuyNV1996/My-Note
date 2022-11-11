## Cài đặt Docker
Kiểm tra xem đã cài phiên bản cũ trên máy chưa và xóa.
```
sudo apt update
sudo apt remove docker docker-engine docker.io 2>/dev/null
```
Kiểm tra update các package mới nhất cài đặt trong máy
```
sudo apt update
```
Cho phép cài đặt các package thông qua HTTPS
```
sudo apt -y install lsb-release gnupg apt-transport-https ca-certificates
```
```
curl software-properties-common
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
```
```
sudo add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
Nếu bạn muốn sử dụng Docker với tài khoản non-root, bạn cần thực hiện thêm user của bạn vào group 'docker'
```
sudo usermod -aG docker $USER
newgrp docker
```
Chạy lenehk kiểm tra docker version
```
docker version
```
```
Client: Docker Engine - Community
 Version:           20.10.18
 API version:       1.41
 Go version:        go1.18.6
 Git commit:        b40c2f6
 Built:             Thu Sep  8 23:11:43 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.18
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.6
  Git commit:       e42327a
  Built:            Thu Sep  8 23:09:30 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.8
  GitCommit:        9cd3357b7fd7218e4aec3eae239db1f68a5a6ec6
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
```
docker info
```
```
docker compose version
```
Kết quả:
```
Docker Compose version v2.10.2
```
Chế độ interactive
```
sudo docker exec -it [containerID] bin/bash
```
```
sudo docker exec -u 0 -it ContainerID bin/bash
```
Nếu bị lỗi E: Method https has died unexpectedly! sử bổ sung GNUTLS_CPUID_OVERRIDE=0x1 vào trước
```
sudo GNUTLS_CPUID_OVERRIDE=0x1 apt-get update
```