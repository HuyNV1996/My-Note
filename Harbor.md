# Cài đặt habor
Download
```
curl -s https://api.github.com/repos/goharbor/harbor/releases/latest | grep browser_download_url | cut -d '"' -f 4 | grep '\.tgz$' | wget -i -
```
Giải nén
```
tar xvzf harbor-offline-installer*.tgz
```
Thay đổi vào thư mục đã tạo sau khi giải nén
```
cd harbor
```
```
cp harbor.yml.tmpl harbor.yml
```
Chỉnh sửa ại file harbor.yml
```
nano harbor.yml
```
Chỉnh sửa như sau:


>...
>#The IP address or hostname to access admin UI and registry service.
>hostname: registry.computingforgeeks.com
>harbor_admin_password: StrongAdminP@s5W0$d

>#Harbor DB configuration
>database:
  password: StrongdbrootP@s5W0$d
...

>Nếu dùng nginx từ bên ngoài server khác trỏ đến
    # Comment port/certificate/private_key
    https:
      # https port for harbor, default is 443
      #port: 443
      # The path of cert and key files for nginx
      #certificate: /your/certificate/path
      #private_key: /your/private/key/path
    ...
    Bỏ comment external_url
    external_url: https://cdn.mandalaholiday.com

>Nếu trỏ thẳng domain vào harbor thì comment external_url và thêm bỏ >comment port:443,certificate,private_key

Chạy lệnh
```
sudo ./prepare
sudo ./install.sh
```
Kết quả

>....
[Step 5]: starting Harbor ...
[+] Running 10/10
 ⠿ Network harbor_harbor        Created                                                                                                                                                          0.1s
 ⠿ Container harbor-log         Started                                                                                                                                                          0.7s
 ⠿ Container registry           Started                                                                                                                                                          1.6s
 ⠿ Container redis              Started                                                                                                                                                          1.4s
 ⠿ Container registryctl        Started                                                                                                                                                          1.2s
 ⠿ Container harbor-portal      Started                                                                                                                                                          1.6s
 ⠿ Container harbor-db          Started                                                                                                                                                          1.3s
 ⠿ Container harbor-core        Started                                                                                                                                                          2.0s
 ⠿ Container nginx              Started                                                                                                                                                          2.5s
 ⠿ Container harbor-jobservice  Started                                                                                                                                                          2.5s
>✔ ----Harbor has been installed and started successfully.----

# Chú ý:
Để xác thực SSL cho domain của harbo. Trước tiên cài nginx trên máy => Tạo project demo đơn giản (Ví dụ hiển thị text html lên màn hình) trỏ domain cần xác thực đến => Chạy lệnh gen ssl => sửa file cấu hình harbor.yaml => tắt nginx => chạy ./prepare và ./insall. Chú ý khi xác thực ssl cần tắt các container tránh việc trùng port 80 giữa nginx ( chạy trực tiếp trên máy) và nginx chạy trên docker.