### Đặt vấn đề
Khi bạn làm một project và import một component vào bên trong một component. Và component này nằm rất sâu so với component dùng nó thì bạn sẽ gặp trường hợp như sau:
```js
import MyComponent from “../../../../../components/MyComponent”
```
Mọi thứ hoạt động hoàn toàn bình thường. NHƯNG nếu bạn move MyComponent đi thì sao? Đoạn code import bên trên chỉ là đường dẫn relative. khi bạn move thì sẽ ảnh hưởng đến toàn bộ component đang dùng MyComponent và việc lúc này là phải đi rà soát lại toàn bộ những nơi dùng component đấy. Mệt phải không?

Giải pháp mang tên path alias
Thay vì dùng đường dẫn relative dài loằng ngoằng như vậy thì chúng ta sẽ định nghĩa một đường dẫn kiểu alias (bạn có thể hiểu là nó như một biến, đại diện cho đường dẫn đến component)

Cấu trúc thư mục như này
```js
src
  |
  components
  |
  helpers
Thêm vào tsconfig.json

{
  “compilerOptions”: {
    //other rules,
    "baseUrl": "./src", // complier sẽ tính gốc bắt đầu từ src
    "paths": {
      "@Components/*": ["./components/*"], // gốc lúc này là src cho nên sẽ là ./components
      "@Helpers/*": ["./helpers/*"],    
    }
  }
}
```
Ngoài ra để webpack có thể hiểu được thì ta cần setup thêm như sau
```js
yarn add --dev tsconfig-paths-webpack-plugin
```

// or
```js
npm install --save-dev tsconfig-paths-webpack-plugin
```
Nếu không có file ta tạo mới file webpack.config.ts và copy đoạn sau vào
```js
// webpack.config.ts
const TsconfigPathsPlugin = require('tsconfig-paths-webpack-plugin');

module.exports = {
  //other rules
  resolve: {
    plugins: [new TsconfigPathsPlugin()],
  }
}
```
Sau đó thay vì như này
```js
import MyComponent from “../../../../../components/MyComponent”
```
Sẽ thành như này

import MyComponent from “@Components/MyComponent”
Và kèm theo đó bây giờ bạn có di chuyển component đấy đi đâu cũng đc, chỉ cần nó vẫn nằm trong folder components là được 😍. Thật là đỉnh phải không?

Example
Bạn có thể tham khảo qua link git này của mình. Muốn chi tiết hơn và đúng với bài này bạn nên tìm commit add path alias

Source
Medium