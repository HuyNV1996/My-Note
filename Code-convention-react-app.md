---
title: 'Một số tip convention giúp app React của bạn được trong lành và dễ thở hơn'
date: '2023-01-12'
tags: ['convention', 'code']
draft: false
summary: 'Một số tip convention giúp app React của bạn được trong lành và dễ thở hơn'
authors: ['default']
---

import Twemoji from './Twemoji.tsx'
import UnsplashPhotoInfo from './UnsplashPhotoInfo.tsx'

![thumbnail-image](/static/images/git-notes.jpg)
<UnsplashPhotoInfo photoURL="https://unsplash.com/photos/842ofHC6MaI" author="Yancy Min" />

#### Đặt tên
* **Tên file** : Sử dụng PascalCase cho tên tệp. Ví dụ **ReservationCard.jsx.**

```js
// bad
import reservationCard from './ReservationCard';

// good
import ReservationCard from './ReservationCard';

// bad
const ReservationItem = <ReservationCard />;

// good
const reservationItem = <ReservationCard />;
```