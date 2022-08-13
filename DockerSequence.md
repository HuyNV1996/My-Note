# DockerSequence

>FROM <image_name> [AS <name>]
>FROM <image_name>[:<tag>] [AS <name>]
>FROM <image_name>[@<digest>] [AS <name>]

* Các bạn có thể hiểu đơn giản, đây là thằng trung tâm mà bạn sẽ sử dụng nó làm cái base cho các action ở phía sau. Một Dockerfile hợp lệ phải bắt đầu bằng lệnh FROM. Bạn có thể sử dụng các images của mình tạo ra, hoặc có thể sử dụng hình ảnh do đồng nghiệp trong team tạo sẵn, hoặc đơn giản hơn là nó được chính thằng Docker build lên.
* FROM có thể xuất hiện nhiều lần trong 1 Dockerfile để tạo nhiều ảnh hoặc sử dụng trong 1 giai đoạn build khác nhau. VỚi mỗi lệnh FROM, sẽ xóa đi trạng thái của FROM được tạo ra ở trước đó.
* Tag và digest là các tùy chọn của image, mặc định nếu không có tag thì nó sẽ lấy version cuối cùng: latest. Example: FROM ubuntu:16:04 AS ubuntu Ở đây, mình đang sử dụng images được Docker build lên có tên là ubuntu, với tag là 16.04, và mình đặt cho nó một cái định danh ngắn gọn là ubuntu. Có một từ khóa nữa mà chắc bạn sẽ gặp nó ở đâu đó rồi, vì nó là thằng duy nhất có thể đứng trước FROM trong Dockerfile, đó là ARG.
ARG thường khai báo các giá trị của biến hỗ trợ cho quá trình build từ FROM. Thường là khai báo tag của image mà mình muốn sử dụng:


docker build -t backend .
docker images
docker container run -p 5000:5000  backend
docker exec -ti <container id> /bin/bash
docker ps