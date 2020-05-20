---
title: Cái nút Unsubscribe
published: true
date: 2018-11-23 10:45:00
tags: random, opinion
description: Chuyện cái nút Unsubscribe và mail spam
image:
---

Một buổi chiều đẹp trời, anh ấy check mail và tình cờ nhìn thấy chức năng `Unsubscribe` mà Google đã khéo léo thêm vào bên dưới mỗi thanh tiêu đề. Thật là một chức năng hữu ích! anh thầm nghĩ, và không ngại ngần dành suốt 5 tiếng đồng hồ tiếp theo để click vào từng cái nút ấy. Thậm chí, anh còn search cả những email spam cũ, đã lâu ngày không thấy gửi tiếp. Đã diệt thì phải trừ tận gốc. Anh nghĩ thế. Xong xuôi đâu vào đấy, anh vươn vai thở phào một cái và tắt máy đi ngủ, lòng tràn đầy vui sướng vì một ngày mai không còn nhìn thấy mail spam.

Sáng ngày hôm sau...

Anh vừa đi vừa chửi, anh chửi thằng Google vì cái nút `Unsubscribe` vô dụng, anh chửi cái hộp mail vì càng lúc càng đầy thêm những mail quảng cáo, anh chửi đám dịch vụ vì ngay khi anh vừa bấm nút unsubscribe, chúng liền gửi thêm hàng tá, hàng tá email quảng cáo vào cái hộp mail xấu số của anh. Tính từ đêm qua đến lúc thức dậy, anh nhận thêm hơn 200 email quảng cáo, có cả những mail đến từ những dịch vụ đã ngừng gửi mail cho anh từ lâu lắm rồi.

Lịt pẹ...

---

Sau nhiều đêm trằn trọc suy nghĩ, anh đi đến một kết luận: Thôi mình đếu xài Gmail nữa vậy.

![](https://media.giphy.com/media/koUtwnvA3TY7C/giphy.gif)

Thay vào đó, là quay lại cách đọc mail của mấy ông già thời cổ đại: **fetch toàn bộ mail về máy tính, rồi dùng [Mutt](http://www.mutt.org/) (nếu thích xài terminal) hoặc [Thunderbird](https://www.thunderbird.net/en-US/) (nếu thích xài GUI) để đọc.**

Mấu chốt của việc trên nằm ở chỗ **fetch toàn bộ mail về máy tính**. Tức là sẽ dùng các công cụ như [offlineimap](http://www.offlineimap.org/) hoặc [mbsync](http://isync.sourceforge.net/mbsync.html) để tải toàn bộ mail từ Google về máy tính, thường là thông qua giao thức IMAP hoặc POP3. Dữ liệu email sẽ được lưu vào máy tính dưới dạng file, các file này thường sẽ được lưu dưới định dạng [Maildir](https://en.wikipedia.org/wiki/Maildir) hoặc [Mbox](https://en.wikipedia.org/wiki/Mbox).

Một khi đã có dữ liệu mail nằm trong máy tính, ta có thể dùng các trình đọc mail có hỗ trợ giao thức Maildir hoặc Mbox, là Mutt hoặc Thunderbird.

Và vì có dữ liệu mail nằm trong máy tính (kể cả các mail spam), ta có toàn quyền xử lý, phân loại email theo ý mình mà không cần phụ thuộc vào Gmail. Đến đây thì chấp cả họ nhà tụi spammer, ta có thể chủ động tìm và xóa đám mail rác ngay trên máy tính.

Để xóa mail rác từ máy tính, có 10000001 cách, ví dụ, đoạn lệnh sau sẽ tìm tất cả các mail có chứa từ khóa `thefullsnack.com` trong thư mục `~/mail/inbox` và đưa hết vào thư mục `~/mail/trash`, để lần sync tiếp theo nó sẽ bị xóa, hoặc thích thì có thể thay bằng lệnh `rm` để xóa nó luôn.

```sh
cd ~/mail/inbox && rg -l -e "thefullsnack.com" | xargs -I {} mv {} ~/mail/trash/
```

Để scale up, có thể viết một script chứa tất cả các regex pattern (vì lệnh `rg` hoặc `ag` sử dụng regex để search) cần lọc, và chạy script này mỗi lần sync email từ server về.

---

Quá nhiều thứ cần làm một cách thủ công... có thể khắc phục bằng cách viết script, dù sao đây cũng là cái giá phải trả để giành được quyền kiểm soát hoàn toàn cái inbox của mình.
