---
title: Chuyện con vịt, cái máy bay và cu Tèo đọc báo
published: true
date: 2017-05-15 15:40:01
tags: math, random, opinion
description: Người đọc tự chịu trách nhiệm về tính xác thực của bài viết.
image: https://thefullsnack.com/posts/img/cu-teo-doc-bao.png
---
Người đọc tự chịu trách nhiệm về tính xác thực của bài viết.

---

Tèo học hết lớp 12 thì bỏ học đi bán báo, vì nhà nghèo không có tiền đi học Đại học như thằng Tony con nhà hàng xóm.

Vì thời buổi Internet phát triển, người ta toàn đọc báo mạng hoặc đọc các tin share đầy trên Facebook. Tèo thất nghiệp, thế là nằm nhà ôm điện thoại vào Facebook hy vọng sẽ kiếm được bài báo nào đăng tin tìm việc để đi làm.

Tình cờ Tèo đọc được một mẫu tin trên một tờ báo nước ngoài, nói về chuyện một máy bay thương mại đang bay trên trời thì tông phải một con vịt. Điều kì lạ là chỉ một con vịt, đâu có bự mấy mà cú tông làm cho máy bay móp luôn phần đầu, còn con vịt thì không toàn thây, máu me thì be bét, thật là tội nghiệp.

Các bạn có thể tự mình [đọc bài báo đó tại đây](http://www.dailymail.co.uk/travel/travel_news/article-3404903/Shocking-photos-huge-hole-left-underside-cabin-passenger-plane-collided-bird-landing.html).

Không kiềm nổi tò mò, Tèo thử làm một phép kiểm chứng đơn giản để xem nếu một con vịt tông vào một chiếc máy bay chở khách thì cú tông đó _mạnh tới cỡ nào_. Tèo bắt tay vào tra Google để tìm những dữ kiện cần thiết.

![](img/airplane.png)

Đầu tiên là chiếc máy bay, Tèo search với từ khóa: **"Average speed of a passenger plane"** để tìm vận tốc trung bình của một chiếc máy bay chở khách đang bay trên trời.

Kết quả thu được là tầm **900 km/h**.

![](img/duck.png)

Tiếp theo, Tèo search **"Average weight of a flying duck"**, vì vốn tiếng Anh không nhiều, Tèo không biết con vịt trời tiếng Anh gọi là gì, thôi thì gọi nó là **con vịt bay** vậy. Google gợi ý ngay cho Tèo con vịt trời gọi là **Mallard**.

Search một hồi Tèo thấy kênh National Geographics có đăng [thông tin cơ bản của một con vịt Mallard](http://www.nationalgeographic.com/animals/birds/m/mallard/). Cân nặng trung bình **1.3 kí lô**, dài chừng **40 cm**. Trong [điều kiện thuận gió](http://www.reelfoot.com/migration_121206.htm), nó có thể bay với tốc độ trung bình là **80 km/h**.

Để biết cú tông mạnh cỡ nào, chúng ta cần tính lực va chạm giữa con vịt và cái máy bay. Thời cấp 3 thì Tèo có học công thức tính lực là:

<math>F = m a </math>

$F$ ở đây chính là **lực va chạm** cần tìm, $m$ là **khối lượng**, tính theo đơn vị kí lô ($kg$) và $a$ là **gia tốc của chuyển động** ($m/s^2$).

Gia tốc trong trường hợp này được tính bằng công thức:

<math> a = \displaystyle\frac{\Delta v}{t} </math>

Trong đó $\Delta v$ là sự thay đổi vận tốc khi va chạm xảy ra. Một con vịt lao đầu với vận tốc **80 km/h** tương đương với **22 m/s**, vào một chiếc máy bay đang bay với vận tốc **900 km/h** tương đương với **250 m/s**, biến thiên vận tốc sẽ được tính như sau:

<math>\Delta v = 250 - (-22) = 250 + 22 = 272\ m/s</math>

$t$ là thời gian xảy ra va chạm, ở đây có thể hiểu là thời gian cho toàn bộ tấm thân con vịt đập hết vào chiếc máy bay. Theo dữ kiện Tèo tìm được thì con vịt dài **40 cm** tương đương với **0.4 m**, lao vào máy bay với vận tốc **250 m/s** thì thời gian xảy ra va chạm sẽ là:

<math>t = \displaystyle\frac{0.4}{272} = 0.0014\ s</math>

Vì ở trên ta đã tính $\Delta v$ theo con vịt nên ta cũng sẽ sử dụng khối lượng của con vịt luôn, $m = 1.3\ kg$.

Từ đó Tèo tính được lực va chạm giữa con vịt và cái máy bay là:

<math>F = m a = 1.3 \times \displaystyle\frac{272}{0.0014} = 194286\ N</math>

Vậy lực va chạm xảy ra khi con vịt xấu số lao đầu vào chiếc máy bay sẽ là **194286 Newton**, tương đương với **19811 kí lô**, hoặc đổi ra sẽ được gần **20 tấn**.

![](img/airplane-duck-collision.png)

Với một lực tác động gần **20 tấn** thì hư hại xảy ra trên máy bay cũng không phải là nhỏ.

Về phía con vịt xấu số, một lực va chạm **20 tấn** là quá lớn để nó có thể chịu đựng được, và kết quả nó không toàn thây là một chuyện rất thương tâm.

Tuy nhiên trên thực tế thì phần thiệt không phải lúc nào cũng nghiên về phía con vịt, nếu con vịt xấu số không đâm đầu vào mũi máy bay mà lại [lao vào động cơ phản lực của cái máy bay](https://www.youtube.com/watch?v=nWTb0QRIt0c), thì nó có thể phá hỏng hoàn toàn động cơ và gây nguy hiểm cho cả chuyến bay.

Viết đến đây thì Tèo cảm thấy rợn người và không dám viết tiếp nữa.

---

Với một lượng thông tin cực kì lớn xuất hiện nhan nhản trên báo chí và trên các mạng xã hội ngày nay, **không ai có thể đảm bảo được thông tin mình đang đọc là hoàn toàn chính xác**, nếu không muốn bị làm một con rối để cho thiên hạ nó dắt mũi lái theo cái gọi là dư luận, chúng ta nên [tự mình kiểm chứng trước khi quyết định bấm nút "Like" hoặc "Share"](http://hyperphysics.phy-astr.gsu.edu/hbase/impulse.html).

Chỉ bằng những phương pháp tìm kiếm đơn giản, hoặc những kĩ năng tính toán được học ngay từ cấp phổ thông, không có gì gọi là quá tầm hiểu biết hoặc khả năng nếu bạn đã học hết lớp 12.

Nói về Tèo, Tèo đã học hết lớp 12, Tèo chưa có việc làm, nhưng Tèo không share bừa bãi lên Facebook. Xin các bạn hãy giống như Tèo.
