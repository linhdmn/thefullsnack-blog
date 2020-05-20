---
title: Tản mạn về nghề đi code thuê
published: true
date: 2017-05-06 15:07:35
tags: random, opinion
description: Cũng lâu rồi mình chưa viết bài mới, dạo này toàn draft mấy bài kĩ thuật dài dòng lê thê thành ra viết nửa chừng toàn tụt hết cảm xúc, thôi thì hôm nay tranh thủ ngồi viết tí random vậy.
image:
---
Cũng lâu rồi mình chưa viết bài mới, dạo này toàn draft mấy bài kĩ thuật dài dòng lê thê thành ra viết nửa chừng toàn tụt hết cảm xúc, thôi thì hôm nay tranh thủ ngồi viết tí [`random`](/tags/random.html) vậy.

---

Bài này nói về những chuyện thường gặp khi làm nghề **gõ code thuê**. Trong đây có một vài từ ngữ không hợp với thuần phong mỹ tục, cân nhắc kĩ trước khi đọc.

_Gõ code thuê tức là code bạn viết ra không thuộc về bạn, mà thuộc về công ty, khách hàng, chủ dự án, người trả tiền cho bạn để bạn code._

**Gõ code thuê** xuất hiện ở mọi nơi, mọi lúc và đối với mọi người. Nói như vậy để các bạn không có cãi, vì dù bạn là nhân viên của một công ty outsourcing, offshore hay công ty làm product, thì chừng nào  người ta vẫn trả tiền cho bạn để bạn code, thì đó là **bạn đi code thuê**.

Và đây là khúc mà vấn đề xuất hiện. Suy nghĩ của số đông những người đi code thuê là: Vì đi code thuê, nên không việc gì chúng ta phải dồn hết tâm huyết, trách nhiệm vào cho nó hết.

Nó xuất hiện dưới 1001 hình dạng khác nhau:

- **Viết code kiểu hack đại cho xong, tạm bợ, overwrite lại code của người khác,...**: Code kiểu này thường là fix được bug rất nhanh, qua mặt được QA và không màn tới ngày mai, vì kiểu gì ngày mai nó cũng sẽ phòi ra bug, hoặc fix xong bug của mình rồi đó, nhưng tự nhiên hôm sau sẽ có thằng nào đó trong team ngồi chửi thề, đơn giản vì cái chỗ fix này làm hỏng phần khác của nó. Kệmẹnóđi, việc của nó mà, đ' phải việc của mình là được rồi. Ví dụ: xài `!important` =)))
- **Hard code**: Code kiểu này cũng rất nhanh, giải quyết vấn đề một cách cực kì chính xác, khỏe quá mà, nhưng nếu ngày mai business logic có thay đổi thì việc update là gần như không thể. Kiểu code 1 giây cho thằng khác maintain mất 1 tháng.
- **Không review code**: Đây là việc mà hầu như team nào cũng có trong quy trình làm việc của mình, nhưng số người làm tốt việc này là rất ít, nếu bạn thường review code một cách nghiêm túc thì xin chúc mừng bạn, và cũng chúc bạn may mắn luôn  khi review code của đồng nghiệp, bảo trọng! Nếu bạn có một thằng khó tính chuyên bắt lỗi mình ở khâu review trong team, xin chúc mừng bạn, đó là người bạn tốt, đừng gây sự với người ta. Nếu bạn thường xuyên review code theo kiểu scroll 1 phát từ đầu trang tới cuối trang rồi bấm nút Approve, merge code, thì xin phép được chửi vào mặt bạn 1 phát, hai cái tính xấu nêu ở trên đều dễ dàng lọt qua được vòng review vì những thằng cẩu thả như bạn. Mình thề là 10 năm đi review code bạn sẽ chẳng học thêm được cái gì từ đồng nghiệp, đừng nói đến chuyện truyền thụ lại được gì cho đàn em.
- **Đổ thừa, không fix những thứ không phải của mình**: Việc bạn thường làm khi bị giao một con bug đó là bật `git blame` để truy xem thằng nào viết cái đoạn code bị lỗi đó, và nếu không phải là bạn thì bạn nhất quyết không fix, cố cãi cho bằng được để cái thằng sinh ra bug dù đang bận cắm đầu ra cũng phải quay lại fix con bug đó. Tưởng tượng bạn đánh DotA và bạn đi top, thằng kia đi bot, và dưới bot đang bị 1vs4, bạn từ chối xuống cứu bot chỉ vì top lane đang không có hero địch nào, farm xả láng và def bot là nhiệm vụ của thằng kia, hay lắm $!& #^ #@&...
- **Chỉ làm đúng requirement, không hơn không kém**: Bạn vào một dự án outsource, anh PM dặn trước với bạn là chỉ làm những gì trong scope, không chiều lòng khách hàng mà làm những thứ ngoài phạm vi những gì mà họ đã thỏa thuận với công ty. Và bạn được giao làm một user story về thanh toán online, trong đó bạn PO có ghi là: user nhập thông tin thẻ tín dụng vào form -> frontend gửi thông tin đó về cho backend để tiến hành thanh toán, không có đề cập tới chuyện có mã hóa hay không, nên bạn không làm dù bạn thừa biết là phải mã hóa thì mới đảm bảo về mặt security, và cũng vì nó ngoài scope, bạn cũng không việc gì phải raise vấn đề này lên trong team hoặc với PO để họ sửa sai, đơn giản vì việc đó nằm ngoài scope, hay lắm $!& #^ #@&...
- ... _còn nhiều lắm_ ...

Và khi tất cả những việc trên xảy ra, thì khách hàng sẽ phàn nàn, thậm chí là mắng chửi, rồi sẽ bắt bạn và cả team đi làm OT vào cuối tuần, rồi các bạn sẽ vừa làm vừa chửi: _"Đ!& #^ bọn tây, bóc lột vcl"_, hoặc chửi bạn PM không biết cách quản lý dự án để cả team phải đi nhận tiền OT như thế này =)))

OK, giờ nhìn lại vấn đề theo một hướng khác:

Tèo là nhân viên của một tiệm bán bánh mì nổi tiếng trong thành phố, vì vậy Tèo cũng là thằng **làm bánh thuê**, vì đi làm thuê nên Tèo không việc gì phải dồn hết tâm huyết vào mỗi ổ bánh mì mà mình làm ra, Tèo có thể cầm điện thoại vào nhà vệ sinh để ngồi làm chuyện đại sự trong 30 phút sau đó không cần rửa tay và đi ra quầy bánh bốc từng mảng patê trét vào ổ bánh mì thịt chả mà bạn đang chờ mua trên đường đi làm mỗi sáng. Bạn nhận lấy ổ bánh mì được gói trong tờ giấy báo Tèo mua lại từ mấy bà bán ve chai, và nhai ngấu nghiến một cách đầy biết ơn, mì hơi dở, nhưng được cái nó rẻ, kệ. Đến trưa thì bạn bị đau bụng (hiển nhiên rồi =))) bạn bực mình chạy tới tiệm và chửi, đơn giản vì bạn bỏ tiền ra mua bánh mì. Tèo bị chửi thì bực tức suốt từ trưa đến tối. Sau vài tháng thì tiệm bánh mì phải đóng cửa vì quá dở và không đảm bảo vệ sinh, Tèo cũng vì thế mà thất nghiệp vì dính phốt "đi ra" từ cái tiệm mất vệ sinh nhất thành phố, không ai dám nhận Tèo vào làm nữa. ¯\\\_(ツ)_/¯

Trích một đoạn trong cuốn **The Pragmatic Programmer**:

> When you do accept the responsibility for an outcome, you should expect to be held accountable for it. When you make a mistake (as we all do) or an error in judgment, admit it honestly and try to offer options.

> Don't blame someone or something else, or make up an execuse. Don't blame all the problems on a vendor, a programming language, management, or your coworkers. Any and all of these may play a role, but it is up to you to provide solutions, not execuses.


Kêt bài: vì là [`random`](/topics/random.html) nên chả có kết bài gì đâu :)) các bạn đọc cho vui thôi nha.
