# Một số lệnh cơ bản thường fungf
Hiển thị thư mục hiện tại
```
pwd
```
Update
```
sudo apt update
```
Trong ubuntu có file /etc/apt/source.list : chứa thông tin các version package đang cài trong máy. Khi chạy câu lệnh update file này sẽ được cập nhật các thông tin phiên bản mới nhất từ internet
Đê xem thông tin file /etc/apt/source.list chạy cat /etc/apt/source.list
```
sudo apt upgrade
```
Lấy thông tin nhưng phần mềm cần update từ file /etc/apt/source.list sau đó kéo về và cài đặt phiên bản mới nhất

Tạo file trong thư mục hiện tại
```
mkdir
```
Di chuyển đến thư mục ...
```
cd
```
Xóa màn hình
```
clear
```

```linux
ls
```
Liệt kê file,folder con trong folder hiện tại

```
tar -cvf sampleArchive.tar /home/sampleArchive
```
Nén file thư mục home/sampleArchive thành file nén có tên sampleArchive.tar
Trong đó -cvf
c - tạo file .tar mới
v - Hiển thị quá trình giải nén lên màn hình
f - Tên file
```
tar -xvf sampleArchive.tar "file1" "file2"
```
Giải nén file tar
```
tar -zxvf sampleArchive.tar.gz "file1" "file2"
```
Giải nén file tar.gz
 ### Cài đặt thông qua file
Sau khi giải nén có file INSTALL
Để cài đặt bấm sudo ./INSTALL
```
./configure
make
sudo make install hoặc sudo make install
```

Kiểm tra port đang listen
```
netstat -tunlp
```
```
sudo rm -rf /var/lib/apt/lists/*
sudo rm -rf /etc/apt/sources.list.d/*
sudo apt-get update
```
# Đổi timezone
```
timedatectl
sudo timedatectl set-timezone Asia/Bangkok
```
# Remote vào máy khác thông qua user/password
```
ssh root@172.255.255.196
```
# Đăng nhập linux thông qua ssh key
- private key: Dùng cho client muốn login vào server
- public key: Lưu tại server theo mục cài đặt, khi login client sẽ gửi private key, server có nhiệm vụ so sánh cặp valide private và public key để cho phép login hay hay không.
### Tạo cặp ssh key
```
ssh-keygen -t rsa
```
Nếu trong root chưa có thư mục .ssh ta tạo dùng lệnh
```
mkdir /root/.ssh
```
Sau đó copy public key vào thư mục cài đặt
```
cp id_rsa.pub /root/.ssh/authorized_keys
```
Thay đổi mod cho các thưc mục chứa public key
/root                               700
/root/.ssh                          700
/root/.ssh/authorized_keys          600
```
chmod 600 /root/.ssh/authorized_keys 
chmod 700 /root/.ssh
chmod 700 /root
```
Nếu tài khoản khác root ví dụ là abc
/home/abc                               700
/home/abc/.ssh                          700
/home/abc/.ssh/authorized_keys          600
```
chmod 600 /home/abc/.ssh/authorized_keys 
chmod 700 /home/abc/.ssh 
chmod 700 /home/abc
```

### Cho phép ssh qua tài khoản root
```
cd /etc/ssh/sshd_config
```
Sửa
FROM:
```
#PermitRootLogin prohibit-password
TO:
PermitRootLogin yes
```
```
sudo systemctl restart ssh
```
Đổi password root do mặc định linux không cho phép ssh qua root
sudo passwd

### Thiết lập đường dẫn ssh public key
AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2
File lưu public key: .ssh/authorized_keys
- Nếu tài khoản root thì đường dẫn là /root/.ssh/authorized_keys
Nếu tài khoản admin thì đường dẫn lư là
/home/admin/.ssh/authorized_keys

### Thêm tài khoản centos
Tạo tài khoản <new_user> bằng lệnh adduser
```
adduser <new_user>
```
Tạo password <new_user> bằng lệnh passwd
```
passwd <new_user>
```
Thêm tài khoản <new_user> vào nhóm sudo cũng sử dụng lệnh adduser
```
usermod -aG wheel <new_user>
```
Kiểm tra lại tài khoản <new_user> xem đã sử dụng được quyền sudo chưa
```
su - <new_user>
sudo ls -la
```