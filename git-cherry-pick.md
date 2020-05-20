---
title: Một commit cho 2 branches - Cherrypick
published: true
date: 2016-09-06 16:11:59
tags: git
description: Giả sử bạn có 2 branches khác nhau cho 2 khách hàng khác nhau, tạm gọi là branch A và branch B. Và bạn đang fix một bug nghiêm trọng cùng tồn tại trên hệ thống của cả 2 khách hàng, bạn muốn chỉ commit một lần nhưng có thể apply vào luôn cả 2 branches.
image:
---
Hôm nay mình gặp phải một trường hợp cần apply cùng một thay đổi cho cả 2 branches khác nhau trong project. 

Giả sử bạn có 2 branches khác nhau cho 2 khách hàng khác nhau, tạm gọi là branch A và branch B. Và bạn đang fix một bug nghiêm trọng cùng tồn tại trên hệ thống của cả 2 khách hàng, bạn muốn chỉ commit một lần nhưng có thể apply vào luôn cả 2 branches.

Bạn có thể dùng lệnh `cherry-pick` của Git để giải quyết, như sau:

```
// Đang ở branch A, commit những thay đổi đó
git add -A
git commit -m "Awesome Bug Fixed :("

// Chuyển qua branch B và thực hiện lệnh cherry-pick 
git checkout B
git cherry-pick A
```

`cherry-pick` sẽ lấy commit cuối cùng ở branch A merge vào branch B.

Ngoài ra, bạn còn có thể chỉ định danh sách các commit cần "bốc" từ A để "bỏ" vào B nếu cần thiết. 

Xem thêm thông tin về `cherry-pick` tại [đây](https://git-scm.com/docs/git-cherry-pick)
