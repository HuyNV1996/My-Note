vps: database account: root/hJ4$Wb#9Y*pTqZ!
### Đăng nhập vào mysql
```sh
mysql -u root -p
```
### Một số query thông dụng
```sh
show databases; #Hiển thị các database hiện có trong MySQL.
use your_db;    #Chuyển sang database mở rồi
exit           #Thoát khỏi MySQL.
create database your_db;
drop database your_db;
```
### Tạo user
```sh
CREATE USER 'huynv_test_user'@'localhost' IDENTIFIED BY 'huynv_test_pw';
```
- Cấp quyền (Gán quyền cho người dùng trên cơ sở dữ liệu mà bạn đã tạo (ví dụ: wordpress). Bạn cần gán quyền để người dùng có thể thực hiện các thao tác cơ bản như đọc và ghi dữ liệu:)
```sh
GRANT ALL PRIVILEGES ON wordpress.* TO 'huynv_test_user'@'localhost';
```
- Làm mới các quyền để đảm bảo rằng các thay đổi được áp dụng ngay lập tức:
```sh
FLUSH PRIVILEGES;
```
- Show các user hiện tại trong db
```sh
SELECT User, Host FROM mysql.user;
```
- Thoát
```sh
exit
```