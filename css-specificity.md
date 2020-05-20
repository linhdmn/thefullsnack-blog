---
title: Hai kiểu lập trình viên
published: true
date: 2018-07-13 15:35:00
tags: random, opinion, css
description: Có hai dạng lập trình viên, một dạng luôn nắm vững lý thuyết, dạng còn lại thì không
image:
---

Luôn có hai dạng lập trình viên, một dạng luôn nắm vững lý thuyết, dạng còn lại thì không.

Những người nắm vững lý thuyết thì luôn có năng suất làm việc cao hơn hẳn những người không học lý thuyết, vì không phải tốn thời gian để thử sai (trial and error), cũng không cần tốn thời gian tra cứu lại kiến thức.

Một ví dụ đơn giản về CSS, chúng ta có class `.gift-image` có thuộc tính `top = 10px`, và chúng ta muốn class này có thuộc tính `top = 0` trên các thiết bị có màn hình nhỏ hơn `600px`:

```
.gift-box {
    .gift-image {
        &.openned {
            position: absolute;
            top: 10px;
            ...
        }
    }
    
    @media (max-width: 600px) {
        .gift-image {
            top: 0;
        }
    }
}
```

Nhưng đoạn code trên sẽ không chạy, và trên mobile, class `.gift-image` vẫn có thuộc tính `top = 10px`.

Một frontend developer không nắm vững kiến thức sẽ fix vấn đề trên như sau:

```
@media (max-width: 600px) {
    .gift-image {
        top: 0 !important;
    }
}
```

Hoặc tốn 10 phút để search Google với một vấn đề không liên quan: `position absolute top not change in media query`.

Ngược lại, một frontend developer nắm vững kiến thức về CSS specificity [^1] [^2] sẽ fix vấn đề trên một cách dễ dàng mà không cần dùng tới `!important`:

```
@media (max-width: 600px) {
    .gift-image.openned {
        top: 0;
    }
}
```

Và thậm chí còn đạt tới cảnh giới code xong không cần test, sure đúng 100%, push lên thẳng master luôn, rồi để cho junior nó fix =)))

Ví dụ trên chưa có phần giải thích, để dành cho anh em đọc xong tự giải thích :relaxed:

**Đọc thêm:**

- [^1]: Calculating a selector's specificity, https://www.w3.org/TR/selectors-3/#specificity
- [^2]: MDN CSS, Specificity, https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity
