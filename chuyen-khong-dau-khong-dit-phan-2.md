---
title: Chuyện không đầu không đít (Phần 2)
published: true
date: 2019-10-04 15:08:00
tags: random, opinion
description:
image:
---
Procrastination, là trạng thái cố tình không làm một việc gì đó, dù biết rõ tầm quan trọng của nó, hậu quả của nó hoặc thậm chí là biết rõ cách thức để hoàn thành công việc đó, nhưng vẫn không chịu động thủ. Ở đây mình chỉ note nhanh ra một vài quan sát dựa trên chủ quan cá nhân về vấn đề này, khi nào có thời gian thì sẽ viết bài chi tiết kèm theo nhiều dẫn chứng vài nguồn tham khảo hơn.

Ví dụ cụ thể, vừa hôm qua đây thôi, sáng ngủ dậy check mail công ty và thấy có vài chuyện làm mình không vừa ý, một bà project manager mà mình rất ghét của một cái team toàn những thằng mình rất ghét gửi một bức mail đề xuất đưa cái feature mà mình đang build suốt tháng nay sang cho tụi nó tiếp quản, vì team mình đang rất nhiều việc mà team nó thì vừa mới hoàn thành dự án rewrite cái trang marketing của công ty sang React với đủ loại animation tứa lưa xài jQuery :unamused:

Toàn một đám làm chuyện ruồi bu :expressionless: trong khi product của công ty thì bug production đầy rẫy không thằng nào chịu fix, toàn đá ra cho cái thằng đang viết bài này hốt, ấy thế mà có thằng đếch nào ghi công đâu.

Thế là đâm ra chán nản và nguyên một ngày chỉ nhìn vào màn hình với mấy dòng code mà không tài nào làm gì được, mặc dù trong đầu vẫn biết rõ ràng cách làm, và biết chỉ thiếu có mươi dòng code nữa thì task sẽ hoàn thành.

Đôi khi kỉ luật cá nhân kém cũng dẫn đến tình trạng procrastination. Ví dụ thay vì đọc sách, bạn không thể cưỡng lại thứ cám dỗ vô hình nào đó và bật Facebook lên để chụp hình check in cuốn sách đó, và cứ mỗi 32 giây bạn mở Facebook lên lại để xem có ai like chưa.

Hoặc cái công việc cần làm nó quá nhỏ tới mức không đáng để bạn quan tâm, hoặc quá rườm rà, ví dụ như việc phải login vào website của công ty điện nước và bấm nút trả bill mỗi tháng.

Vậy, làm cách nào để vượt qua trạng thái procrastination?

Chỉ có một cách, đó là làm nó ngay lập tức. Nếu việc cần làm chỉ tốn có 5 phút, làm ngay trong 5 phút đó. Nếu việc đó phức tạp hơn, hãy bắt đầu bằng việc viết ra các bước cần thiết để thực hiện nó, viết ở đâu cũng được, nếu có thể thì viết ra một tờ post-it rồi dán lên trán để tăng visibility. Nếu đó là code, thì viết một dòng `TODO`, comment, hoặc pseudo code, ví dụ:

```javascript
// comment version
function getUserById(id) {
  // - create a DB connection instance
  // - call connect funciton
  // - if ok, query the user that matched the id
  // - if not, throw error
}

// pseudo code version
function getUserById(id) {
  // - const db = call create db instance function
  // - if (db.connect()) then
  // -   query user from db
  // - else throw error
}
```

Mình thì thích viết pseudo code hơn, vì nó ngắn gọn. Có nhiều khi mình viết pseudo code xong thì nhận ra chỉ cần bỏ comment đi là nó chạy được luôn :sunglasses:
