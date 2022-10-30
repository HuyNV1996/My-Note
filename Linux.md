# Linux
```

```
```
pwd
```
Hiển thị thư mục hiện tại
```
sudo apt update
```
Trong ubuntu có file /etc/apt/source.list : chứa thông tin các version package đang cài trong máy. Khi chạy câu lệnh update file này sẽ được cập nhật các thông tin phiên bản mới nhất từ internet
Đê xem thông tin file /etc/apt/source.list chạy cat /etc/apt/source.list
```
sudo apt upgrade
```
Lấy thông tin nhưng phần mềm cần update từ file /etc/apt/source.list sau đó kéo về và cài đặt phiên bản mới nhất
```
mkdir
```
Tạo file trong thư mục hiện tại
```
cd
```
Di chuyển đến thư mục ...
```
clear
```
Xóa màn hình
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