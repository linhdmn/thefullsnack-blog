# Lỗi của thằng "đánh code"

Mấy hôm nay nếu ai đọc báo sẽ thấy xôn xao về chuyện người dân Hawaii nhận được tin nhắn báo động có tên lửa tấn công, rất may tin này là giả. ([nguồn: trên báo](https://www.washingtonpost.com/news/post-nation/wp/2018/01/13/hawaii-residents-get-ballistic-missile-threat-messages/?tid=a_inl&utm_term=.b4c72313e3a2)). 

Nguyên nhân của vụ việc này, có lẽ đối với nhiều người, nghe nó rất là hài hước: **Anh nhân viên trực tổng đài bấm lộn nút!**.

<blockquote class="twitter-tweet tw-align-center" data-lang="en"><p lang="en" dir="ltr">Hawaii missile alert: How one employee &quot;pushed the wrong button&quot; and caused a wave of panic <a href="https://t.co/sO4gyNQQTD">https://t.co/sO4gyNQQTD</a></p>&mdash; Washington Post (@washingtonpost) <a href="https://twitter.com/washingtonpost/status/953013684649037827?ref_src=twsrc%5Etfw">January 15, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Nhiệm vụ của anh này mỗi sáng là bấm nút test hệ thống báo động, cơ mà thật khốn nạn làm sao, khi mà `nút test` và `nút báo động thật` nó nằm cạnh nhau...

> From a drop-down menu on a computer program, he saw two options: “Test missile alert” and “Missile alert.” He was supposed to choose the former; as much of the world now knows, he chose the latter, an initiation of a real-life missile alert.
> 
> - Nguồn: [The Washington Post](https://www.washingtonpost.com/news/post-nation/wp/2018/01/14/hawaii-missile-alert-how-one-employee-pushed-the-wrong-button-and-caused-a-wave-of-panic/?utm_term=.b474f37e696d)

Về chuyện này thì lỗi thuộc về ai? Giới chức sắc, lãnh đạo thì đang blame về quy trình, giới bình dân sẽ blame thằng `đánh code` làm ra cái phần mềm đó, và giới hành nghề `đánh code`, có lẽ vì là người trong nghề, nên sẽ blame thằng designer, thằng product manager hoặc bất cứ thằng nào nghĩ ra cái ý tưởng để hai cái nút cạnh nhau, thằng nào cũng được, miễn không phải là thằng `đánh code`.

Bạn blame thằng nào?

---

Từ hồi mới đi làm đến giờ, đã có hàng tỉ lần mình đóng vai thằng `đánh code` trong những tình huống đang ngồi làm thì gặp những chỗ trông rất khó coi, có lúc thì là một con bug nào đó được giấu đi, có lúc thì là cái UI khó hiểu đối với dân non-tech (và cũng là targeted user của project),...

Những lúc như thế thì thường phản ứng của mình (hoặc của team, hoặc của anh PM, TM,... các kiểu):

- Requirement nó đưa ra như vậy, làm theo thôi, quan tâm làm gì.
- Kệ bà nó đi, có phải việc của mình đâu mà lo.
- Việc của chú là làm theo design, còn design như thế nào là việc của tụi client, chú đừng có dài tay, cái này không có trong scope của project đâu.

Quèo, thì thôi vậy.

Nhưng nếu mà nghĩ kĩ ra, thấy sai mà không sửa, cứ một mực làm theo chỉ vì requirement nó viết như vậy, hoặc vì chuyện nó hoạt động ra sao không phải là việc của mình, nghe nó giống như kiểu một anh đầu bếp, lấy nguyên liệu hết hạn sử dụng để nấu ăn cho công nhân trong một nhà máy, rủi mà nguyên cả nhà máy ăn vào rồi ngộ độc hết, thì hành động đó là tội ác, không thể bào chữa bằng cách nói tại ông chủ nhà máy cố tình mua đồ hết hạn, anh đầu bếp biết nhưng không thể làm khác đi được.

---

Mình cá là anh chàng `đánh code` trong cái dự án phát triển phần mềm báo động trên kia cũng từng dừng lại và tự hỏi có nên làm theo cái yêu cầu quái đản (đặt 2 cái nút cạnh nhau) kia không? Và vì một lý do gì đó, anh ấy vẫn làm theo, vậy thì lỗi hoàn toàn là ở anh ta rồi.

Cá nhân mình thì may mắn hơn anh chàng kia một chút, vì những sản phẩm mình từng tham gia và làm việc theo cái kiểu trên kia, cho đến thời điểm này chắc không còn tồn tại nữa, nếu không chắc mình sẽ sống cả đời trong lo sợ và hối hận quá :joy:.

--@TAGS: random, opinion
