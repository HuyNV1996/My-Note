```
crontab -e
```
```
* * * * * /bin/ls /đường/dẫn/thư/mục > /đường/dẫn/tới/tệp-log 2>&1
```
Ví dụ: 
```
*/10 * * * * python /root/luckystudio/iwealthclub/tcbs.py  >/dev/null 2>&1
```

Kiểm tra cronjob
```
grep CRON /var/log/syslog
```
Tham khảo một số cronjob generator online