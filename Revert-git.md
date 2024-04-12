Khi commit chạy bị lỗi => muốn back lại version trước.
Bước 1: check commit id bằng cách chạy
```sh
git log
```
- Nó sẽ liệt kê toàn bộ lịch sử commit, merge => lấy commit id muốn back về.
Bước 2: Chạy lệnh
```sh
git reset --hard <commit-id> 
```
hoặc
```sh
git revert <commit-id>
```