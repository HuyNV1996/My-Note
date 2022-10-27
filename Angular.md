# ANGULAR
Thành phần trong App.Modules
```js
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent }  from './app.component';

@NgModule({
  imports:      [ BrowserModule ],
  declarations: [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```
Ví dụ: Xây dựng app hiện thị đánh giá * trong phim.
Các chức năng:
* Display movie information
* Provide user profiles
* Fetch movie information
* Allow rating of movies
* Recommend new movies
