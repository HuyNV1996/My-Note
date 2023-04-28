Triển khai Dotnet Core lên Ubuntu
Bước 1: Cài Dotnet sdk, dotnet runtime
```
sudo apt install aspnetcore-runtime-6.0=6.0.8-1 dotnet-apphost-pack-6.0=6.0.8-1 dotnet-host=6.0.8-1 dotnet-hostfxr-6.0=6.0.8-1 dotnet-runtime-6.0=6.0.8-1 dotnet-sdk-6.0=6.0.400-1 dotnet-targeting-pack-6.0=6.0.8-1
```
Bước 2: Tạo file định nghĩa serivce
```
sudo nano /etc/systemd/system/uat-mandalaholiday.service
```
Bổ sung vào file cấu hình
```
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/uat-mandalaholiday
ExecStart=/usr/bin/dotnet /var/www/uat-mandalaholiday/MandalaBooking.Full.HttpApi.Host.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```
Bước 3: Thêm file code build vào thư mục /var/www/uat-mandalaholiday
Bước 4: start service
```
sudo systemctl enable uat-mandalaholiday.service
sudo systemctl start uat-mandalaholiday.service
```
Bước 5: Kiểm tra
```
sudo systemctl status uat-mandalaholiday.service
```