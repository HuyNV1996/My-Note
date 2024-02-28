# Cài đặt apache
```linux
sudo yum update httpd
sudo yum install httpd
```
### Mở firewall
```
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```
### Check browser
```bash
sudo systemctl start httpd
sudo systemctl status httpd
```
### Check virtual host
```bash
hostname -I
```
### Sử dụng curl
```
curl -4 icanhazip.com
```
hoặc
```bash
http://your_server_ip
```
Một số lệnh apache phổ biến
```
systemctl status httpd.service
sudo systemctl restart httpd
sudo systemctl disable httpd
sudo systemctl enable httpd
sudo systemctl reload httpd
sudo systemctl start httpd
```
remove comlete apache.service
```
# Xóa apache
Query and list all the Apache rpm packages
```
rpm -qa "httpd"
```
2)List the installed Apache packages.
```
yum list installed "httpd*"
```
3)Remove the installed httpd packages
```
yum remove "httpd*" -y
```
5)Remove the Configuration Files\
```
rm -rf /etc/httpd
```
6)Remove the Supporing files and httpd modules
```
rm -rf /usr/lib64/httpd
```
7)delete the Apache user
```
userdel -r apache
```
8)check the apache user in /etc/passwd location.
grep "apache" /etc/passwd
9)Check the status of Apache server
systemctl status httpd
```

# Deploy website
### Tạo folder
```
sudo mkdir -p /var/www/your_domain/public
sudo mkdir -p /var/www/your_domain/log
sudo chown -R $USER:$USER /var/www/your_domain/public
```
### Phân quyền cho folder
```
sudo chown -R $USER:$USER /var/www/your_domain/public
sudo chmod -R 755 /var/www
```
### Tạo file html
```html
<html>
  <head>
    <title>Welcome to your website!</title>
  </head>
  <body>
    <h1>Success! The your_domain virtual host is working!</h1>
  </body>
</html>
```
```
sudo mkdir /etc/httpd/sites-available /etc/httpd/sites-enabled
sudo no /etc/httpd/conf/httpd.conf
```
Thêm IncludeOptional sites-enabled/*.conf vào file
```
sudo nano /etc/httpd/sites-available/your_domain.conf
```
Sửa
```
<VirtualHost *:80>
    ServerName www.annamrailway.com.vn
    ServerAlias annamrailway.com.vn
    DocumentRoot /var/www/annamrailway.com.vn/public
    DirectoryIndex index.php index.html
    ErrorLog /var/www/annamrailway.com.vn/log/error.log
    CustomLog /var/www/annamrailway.com.vn/log/requests.log combined
RewriteEngine on
RewriteCond %{SERVER_NAME} =annamrailway.com.vn [OR]
RewriteCond %{SERVER_NAME} =www.annamrailway.com.vn
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```
```
sudo ln -s /etc/httpd/sites-available/your_domain.conf /etc/httpd/sites-enabled/your_domain.conf
```
Chính sửa quyeenf SELinux cho Virtual Hosts
```
sudo setsebool -P httpd_unified 1
sudo ls -dZ /var/www/your_domain/log/
sudo semanage fcontext -a -t httpd_log_t "/var/www/your_domain/log(/.*)?"
sudo restorecon -R -v /var/www/your_domain/log
sudo ls -dZ /var/www/your_domain/log/
sudo systemctl restart httpd
ls -lZ /var/www/your_domain/log
```
Testing!!!!!!!!

# Gen SSL Apache
```
sudo yum install epel-release
```
```
sudo yum install certbot python2-certbot-apache mod_ssl
```
```
sudo certbot --apache -d example.com
```
hoặc
```
sudo certbot --apache -d example.com -d www.example.com
```
```
sudo certbot --apache
```
