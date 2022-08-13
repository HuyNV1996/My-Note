# SqlServerDocker

step 1: docker pull mcr.microsoft.com/mssql/server:2019-CU16-GDR1-ubuntu-20.04
step 2:
-d: Detact( background) mode
-e: environment variable
--name: container's name
-p: map port to container

docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Abc@123456789" -p 1435:1433 --name sql-server-2019-container -d mcr.microsoft.com/mssql/server:2019-CU16-GDR1-ubuntu-20.04

docker ps
docker ps -a
docker stop --name's container/id's container


Mở bash shell trong container

Cú pháp: docker exec -it <container_id> /bin/bash
Ví dụ:
# docker ps -a
CONTAINER ID     IMAGE       COMMAND    CREATED        STATUS    PORTS NAMES
62c3c7efa8be ubuntu:14.04 "/bin/bash" 45 seconds ago Up 5 seconds nostalgic_cori
# docker exec -it 62c3c7efa8be /bin/bash
root@62c3c7efa8be:/# whoami
root
root@62c3c7efa8be:/# hostname
62c3c7efa8be
root@62c3c7efa8be:/#