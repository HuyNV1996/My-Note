Bật/tắt Oracle

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
