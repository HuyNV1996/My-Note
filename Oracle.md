### Lỗi sqlplus not fount
```
su - oracle
```
```
source oraenv
```
(Enter SID)

```
sqlplus / as sysdba
```
### Thay đổi user sys
```
alter user sys identified by <yourpassword>;
ALTER USER  IDENTIFIED BY new_password;
```
### Tạo user mới
```
create user user_name identified by admin_password;
```
### Assign quyền sysdba
```
grant sysdba to user_name;
grant create session to user_name;
```
```
alter session set "_ORACLE_SCRIPT"=true;
```
### Kết nối với sql developer
Kiểm tra lsnrctl
```
lsnrctl status
```

### Bật/tắt Oracle

```
sqlplus / as sysdba
```

```
SQL> STARTUP
```

Nếu thành công
ORACLE instance started.

```
Total System Global Area  599785472 bytes
Fixed Size                  1220804 bytes
Variable Size             180358972 bytes
Database Buffers          415236096 bytes
Redo Buffers                2969600 bytes
Database mounted.
Database opened.
```

Xác nhận database đã started

> SQL> select count(\*) from hr.employees;

Để thoát SQL Command Line

> SQL> EXIT

Tắt database

> SQL> SHUTDOWN IMMEDIATE

```
Database closed.
Database dismounted.
ORACLE instance shut down.
```

Bật tắt Oracle service
```
#su - oracle
#dbstart
#lsnrctl start
```
