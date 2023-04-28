### Giới thiệu chung
Trải qua hơn 20 năm giới thiệu đến cộng đồng, Javascript đã trở lên phổ biến nhất trong các ngôn ngữ hiện nay. Ban đầu chúng là những script nhỏ, về sau chung đóng vai trò quan trọng trong việc xây dựng webpage. Js đã phát triển và trở thành 1 sự lựa trọng tốt cho cả frontend và backend. Khi mà kích thước, phạm vi, mức độ phức tạp của các chương trình Javascript ngày càng rộng lớn. Việc kiểm soát các khai báo, môi liên hệ giữa các unit of code ngày càng phức tạp, khó kiểm soát. Việc debug trở lên khó khăn khi Js là 1 ngôn ngữ với 'type' động. Chính vì những khó khăn đó TypeScript đã ra đời để khắc phục.
Dynamic types với javascript
Javascript là ngôn ngữ với 'type' động, có nghĩa là chúng ta có thể thay đổi 'datatype' - kiểu dữ liệu.
Ví dụ
```js
let name ='abc'
let name = 35
let name = true
```
Ta có thể thấy biến name thay đổi kiểu giữ liệu liên tục (type động) mang tới freedom, tự do, mất kiểm soát kiểu biến. Sẽ cho ra kết quả sai nếu như truyền vào tham số bị sai
=> Typescript sinh ra để khắc phục các nhược điểm trên.

Ví dụ về js:
```js
const sum = (x,y) => {
    return x+y;
}
```
```js
console.log('>>>> check sum , sum('name',10))
```
=> Kết quả là name10. Nó sẽ tự động hiểu là 2 string cộng lại với nhau. Không báo lỗi nhưng nó sẽ ra kết quả sai so với mong muốn của chúng ta.

Ví dụ về ts:
```js
const sum = (x: number, y: number) =>{
    return x+y
}
```
console.log('>>>> check sum , sum('name',10)) 
=> Chưa cần chạy dự án trình compiler nó sẽ tự báo lỗi

=> Typescript giúp cho việc kiểm soát dễ hơn