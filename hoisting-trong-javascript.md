---
title: ""Hoisting trong javascript"
date: '2023-03-20'
draft: false
summary: 'Key notes to save plenty of time while working with git for command-line-Yarn và npm là mã nguồn mở, giúp chúng ta quản lý các thư viện code mà dự án sử dụng.'
# images: ['/static/images/git-notes.jpg']
authors: ['default']
tags: ['npm', 'yarn', 'package-manager']
---
import Twemoji from './Twemoji.tsx'
import UnsplashPhotoInfo from './UnsplashPhotoInfo.tsx'

![thumbnail-image](/static/images/git-notes.jpg)
<UnsplashPhotoInfo photoURL="https://unsplash.com/photos/842ofHC6MaI" author="Yancy Min" />
#### Định nghĩa
**hoisting** là cơ chế của JavaScript cho phép **các khai báo biến** hoặc hàm được dời lên trên đầu phạm vi của chúng trước khi thực thi đoạn code.
**Lưu ý**: Là cơ chế này chỉ di chuyển phần khai báo mà thôi còn các phần khác giữ nguyên không đụng gì đến nó hết.
##### Ví dụ 1:

```js
name = 'huy nguyen'
console.log('xin chào',)
var name
```
kết quả
> xin chào huy nguyen

=> Kết luận: biến name tự động được đẩy lên trên đầu theo cơ chế hoistring.

##### Ví dụ 2:

```js
console.log('xin chào',)
var name = 'huy nguyen'
```
kết quả
> xin chào null

=> khai báo name tự động di chuyển lên trên đầu theo cơ chế hoistring, tuy nhiên giá trị sẽ không di chuyển lên theo. Bản chất sau khi thực hiện cơ chế hoistring code sẽ trở thành

```js
var name
console.log('xin chào',)
name = 'huy nguyen'
```
**Lưu ý**: Để tránh trường hợp trên chúng ta nên vừa khai báo, vừa gán giá trị 
```js
var name = 'huy nguyen'
```
#### Ví dụ 3:
```js
// Gọi hàm trước khi khai báo
xin_chao();
function xin_chao(){
    console.log('xin_chao');
}
```
Sau khi thực hiện xong cơ chế hoisting nó sẽ trở thành
```js
function xin_chao(){
    console.log('xin_chao');
}
// Gọi hàm trước khi khai báo
xin_chao();
```
=> Kết luận: Hàm cũng tư tự như biến, cũng hoạt động theo cơ chế hoisting

#### Ví dụ 4:
```js
// Gọi hàm trước
hoi();

// Khai báo, khởi tạo biểu thức hàm
var hoi = function() {
    var message = "Lập trình JavaScript căn bản";
    document.write(message);
};
```
Kết quả:
> Uncaught TypeError: hois is not a function

Kết luận
> Vì biểu thức hàm bản chất là hàm được gán cho một biến. Do đó, nó cũng giống như vừa khai báo và vừa khởi tạo biến.
> Thế nên, cơ chế hoisting không áp dụng cho biểu thức hàm.