Nếu bạn muốn có một giải pháp thay thế để cài đặt SSL cho domain "booking.dev" trên localhost với Kubernetes và ingress, bạn có thể sử dụng mkcert. mkcert là một công cụ đơn giản để tạo chứng chỉ SSL cho các domain cục bộ mà trình duyệt của bạn sẽ tin tưởng mà không cần cảnh báo. Dưới đây là các bước chi tiết để thực hiện điều này.

Bước 1: Cài đặt mkcert
Cài đặt Chocolatey (nếu chưa có):

Mở PowerShell với quyền admin và chạy:
sh
Sao chép mã
Set-ExecutionPolicy Bypass -Scope Process -Force; `
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
Cài đặt mkcert:

Sau khi cài đặt Chocolatey, chạy lệnh sau trong PowerShell hoặc Command Prompt với quyền admin:
sh
Sao chép mã
choco install mkcert
Bước 2: Tạo và Cài đặt Local CA (Certificate Authority)
Tạo và cài đặt CA:
sh
Sao chép mã
mkcert -install
Bước 3: Tạo Chứng Chỉ cho booking.dev
Tạo chứng chỉ cho booking.dev:

sh
Sao chép mã
mkcert booking.dev
Lệnh này sẽ tạo ra hai file: booking.dev-key.pem và booking.dev.pem.

Bước 4: Tạo Kubernetes Secret
Tạo Secret:
sh
Sao chép mã
kubectl create secret tls booking-dev-tls --cert=booking.dev.pem --key=booking.dev-key.pem
Bước 5: Cấu Hình Ingress
Tạo file ingress.yaml với nội dung như sau:

yaml
Sao chép mã
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: booking-dev-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
  - hosts:
    - booking.dev
    secretName: booking-dev-tls
  rules:
  - host: booking.dev
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: booking-service
            port:
              number: 80
Triển khai Ingress:

sh
Sao chép mã
kubectl apply -f ingress.yaml
Bước 6: Cập Nhật File Hosts
Mở file hosts:

Trên Windows, mở Notepad với quyền admin và mở file C:\Windows\System32\drivers\etc\hosts.
Thêm Dòng Sau:

plaintext
Sao chép mã
127.0.0.1 booking.dev
Bước 7: Truy Cập booking.dev
Bây giờ bạn có thể truy cập https://booking.dev trên trình duyệt của mình mà không gặp vấn đề về chứng chỉ SSL, vì mkcert đã thêm chứng chỉ tự ký vào danh sách chứng chỉ tin cậy của hệ điều hành.

Tóm Tắt
Cài đặt mkcert bằng Chocolatey.
Tạo và cài đặt Local CA bằng mkcert -install.
Tạo chứng chỉ SSL cho booking.dev bằng mkcert booking.dev.
Tạo Kubernetes Secret từ các file chứng chỉ.
Cấu hình Ingress để sử dụng Secret.
Cập nhật file hosts để trỏ booking.dev về 127.0.0.1.
Với cách này, bạn có thể dễ dàng tạo và sử dụng chứng chỉ SSL cho các domain cục bộ mà không gặp các cảnh báo bảo mật trên trình duyệt.