#### Cài đặt mariadb
```
	sudo apt update && sudo apt upgrade -y
	sudo apt install curl apt-transport-https software-properties-common lsb-release ca-certificates gnupg2
	sudo apt install mariadb-server
	sudo systemctl start mariadb.service
```
Đặt mật khẩu root
```
sudo mysql_secure_installation
```
Vào file /etc/mysql/mariadb.conf.d/ 50-server.cnf sửa bind-address = IP address của host
Gõ 
```
mariadb
```
Tạo user
```
CREATE USER 'mariadb_uat'@'%' IDENTIFIED BY 'Apec@123';
```
Cấp quyền
```
GRANT REPLICATION SLAVE ON *.* TO 'mariadb_uat'@'%';
```
Refresh
```
FLUSH PRIVILEGES;
```
Kiểm tra
```
SELECT host, user, password FROM mysql.user;
```
Kết nối với tools
