https://www.markdownguide.org/basic-syntax/
## Bootstrap
### Margin- padding
**Thiết lập margin**

>m{sides}-{size} hoặc m{sides}-{breakpoint}-{size}

**Padding** 
>p{sides}-{size}

>-sides thay bằng t|l|r|b|x|y tương ứng với top, left, right, bottom, left-right, top-bottom. Nếu sides để trống nghĩa là thiết lập cho cả bốn phía (ví dụ m-0, p-2

>-sizes thay bằng các số 0 1 2 3 4 5 hoặc auto để thiết lập kích thước (0, 0.25, 0.5, 1, 1.5, 3) rem

breakpoint tương ứng với điểm chia màn hình sm, md, lg, xl hoặc để trống

**Ví dụ:** px-md-4 thực hiện tạo padding cỡ 4 (1.5rem) cho cạnh trái, phải tương ứng màn hình md

### Grid Layout
>- Extra small < 576px => .col-
>- Small >= 576px =>.col-sm- 
>- Medium >= 768px =>.col-md- 
>- Large >= 992px =>.col-lg- 
>- Extra large >= 1200px =>.col-xl-
### Display
.d-{breakpoint}-{value} ví dụ .d-none, .d-sm-inline, .d-lg-flex...
Thiết lập hiện thị phần tử dùng .visible
Thiết lập ẩn phần tử dùng .invisible

### Float
Thuộc tính CSS float nhận các giá trị **left, right, none** có thể thiết lập trong Bootstrap với các class có cấu trúc

.float-{breakpoint}-{value} ví dụ .float-none, .float-sm-right, .float-lg-left...

Muốn hủy thuộc tính float của một phần tử dùng class **clearfix**

### Postion
Đó là các class: 
>.position-(static|relative|absolute|fixed|sticky), .fixed-top, .fixed-bottom

**Ví dụ:** postion-relative
### Shadow
> - .shadow đổ bóng cỡ trung bình
> - .shadow-sm đổ bóng cỡ nhỏ
> - .shadow-lg đổ bóng cỡ lớn
> - .shadow-none bỏ đổ bóng
### width, height
> - w-25 w-50 w-75 w-100 w-auto để thiết lập chiều rộng theo phần trăm của chiều rộng phần tử cha, auto là tự động co
> - mw-100 thiết lập thuộc tính max-width: 100%
> - h-25 h-50 h-75 h-100 h-auto để thiết lập chiều cao theo phần trăm của chiều cao phần tử cha, auto là tự động co
> - mh-100 thiết lập thuộc tính max-height: 100%

### Nhúng video
<div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" 
            src="https://www.youtube.com/watch?v=UtF6Jej8yb4&list=PLKLOoPAtLjyLMlIejDx47hfdQtvzhL-Be&index=5" allowfullscreen></iframe>
    </div>

### Flex Box
> .d-flex hoặc  .d-{breakpoint}-flex hoặc {breakpoint}-inline-flex
#### flex-wrap
>.flex-{breakpoints}-{wrap}

Ví dụ:
>.flex-nowrap, .flex-wrap, .flex-reverse
.flex-sm-nowrap, .flex-lg-wrap, .flex-md-reverse ...

####  flex-direction
> .flex-{direction} hoặc .flex-{breakpoint}-{direction}

Ví dụ:

>.flex-row, .flex-column, .flex-column-reverse, .flex-row-reverse
>.flex-xs-row, .flex-sm-column, .flex-lg-column-reverse, .flex-md-row-reverse ...

#### justify-content
Cú pháp:
>justify-content-{values} hoặc justify-content-{breakpoint}-{values}

Ví dụ: 
>justify-content-end
>justify-content-center

#### align-items
Cú pháp:
>.align-items-{values} hoặc .align-items-{breakpoints}-{values}

trong đó values là các giá trị **stretch, center, baseline, around, between**

#### align-content
Cú pháp:
>align-content-{values} hoặc .align-content-{breakpoint}-{values}

Với {values} là **start, end, center, around, stretch**

