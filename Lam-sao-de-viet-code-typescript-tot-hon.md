### Không sử dụng any
```js
fuction sum(a: any, b: any){
    return a+b;
}
```
Viết lại
```js
fuction sum(a: number, b: number){
    return a+b;
}
```
Nếu sử dụng any với các tham số đầu vào khiến cho code thiếu chặt chẽ, đôi khi nó sẽ làm cho kết quả trả về không đúng với mục đích xây dựng method.

### Luôn định nghĩa kiểu trả về cho các function
Không nên:
```js
fuction sum(a: number, b: number){
    return a+b;
}
```
Viết nên:
```js
fuction sum(a: number, b: number): number{
    return a+b;
}
```
### Sử dụng một số type checking cài đặt trong tsconfig
https://www.typescriptlang.org/tsconfig
Ví dụ để check số lương tham số vào và các tham số được sử dụng tánh việc viết code bị quên ta sẽ bổ sung rules checking vào file tsconfig.ts trong mục sau
```ts
"compilerOptions": {
    "noUnusedParameters": true
  }
```