---
title: Bàn về Problem Solving Skill
published: true
date: 2018-10-03 19:15:23
tags: random, opinion
description: Phỏng vấn thì tào lao hết sức, tuyển senior mà bắt test thuật toán, làm 1 đống bài test khó trong vòng 1h...
image:
---
Hôm nay tình cờ lọt vào một cái động tào lao, bắt gặp một anh bạn nào đó, có lẽ là mới đi phỏng vấn một vị trí senior dev nào đó, rồi đăng lên cái dòng này:

> Phỏng vấn thì tào lao hết sức, tuyển senior mà bắt test thuật toán, làm 1 đống bài test khó trong vòng 1h 

Đoán chắc là anh bạn này phỏng vấn không được tốt cho lắm, điều này cũng dễ hiểu, vì chỉ cần đọc cái dòng trên thì cũng đủ thấy, anh bạn này đã apply nhầm vị trí. Với tất cả sự thiển cận, thiếu hiểu biết và non nớt cả về tư duy lẫn tư cách như thế, anh ta hoàn toàn không thể là một senior developer, hoặc nếu có đang mang danh senior ở một công ty nào đó, thì chẳng qua công ty đó đã dán cho cái mác senior, với một mục đích duy nhất là làm anh bạn đó trở nên có giá trị hơn trong mấy cái hợp đồng outsourcing của công ty. Thật là bất hạnh cho cả hai phía.

~Thế đéo nào~ Tại vì sao, mà một senior developer lại có thể cảm thấy bị xúc phạm và phản ứng đến mức đó khi bị hỏi về thuật toán trong một cuộc phỏng vấn? Phải chăng là anh ta không hiểu lý do đằng sau của việc kiểm tra bằng thuật toán? Và chính xác thì anh bạn đó expect được phỏng vấn cái gì nếu không phải là thuật toán? Hay là muốn được hỏi về sự khác nhau về mặt cú pháp giữa ES6 và ES5 trong JavaScript??? Hỏi mấy cái đó để làm ~đéo~ gì?

Đọc thêm một chút thì lại thấy:

> Lúc gọi điện mình bảo không biết ruby on rails, Nhân sự khẳng định không quan trọng ngôn ngữ, quan trọng thuật toán =))

À, thì ra không những người đi phỏng vấn không biết mục đích, mà cả nhân sự cũng không rõ vì sao các engineer của công ty mình lại đặt ra các câu hỏi về thuật toán. Thế còn các bạn engineer đã và đang làm công việc phỏng vấn? Tại sao các bạn lại hỏi thuật toán?

---

Lại nói về công việc hằng ngày của một developer, là gì? build thêm feature mới, hoặc build một sản phẩm hoàn toàn mới, nhưng cũng có thể là bảo trì (maintenance - tên gọi hoa mỹ của những hành động: fix bug, refactoring,...) sau khi sản phẩm được released. Dù bạn làm gì, thì cũng đều có thể gói gọn trong các bước sau:

- **Tiếp nhận và hiểu vấn đề:** ở dây có thể là bug description, hoặc là yêu cầu của sản phẩm/feature,...
- **Phân tích vấn đề:** nếu là bug, đây là bước điều tra các hiện tượng, tìm cách tái hiện (reproduce) lại bug đó, tìm hiểu nguyên nhân gốc rễ của vấn đề (root cause) và hướng giải quyết, nếu là build mới, đây là bước phân tích các yêu cầu kĩ thuật, search google để tìm các thư viện hỗ trợ nếu có (vì mình thường thấy nhiều bạn làm gì cũng search thư viện có sẵn, nên mình đoán cái đó nằm trong quy trình này, nên ghi vô luộn).
- **Lên phương án hành động:** sau khi đã có đầy đủ các dữ kiện cần thiết, hiểu rõ được bản chất của vấn đề và hình dung được hướng giải quyết, thì chúng ta cần vạch ra kế hoạch từng bước để hiện thực hóa giải pháp đó, quan trọng hơn nữa, là phương án hành động này phải đảm bảo chỉ giải quyết vấn đề cần giải quyết :joy: mà không làm ảnh hưởng đến những thứ khác.
- **Thực hiện:**  get :shit: done
- **Rút kinh nghiệm:** sau khi đã giải quyết xong vấn đề, đây là bước cực kì quan trọng, nhìn lại quá trình thực hiện và những sai lầm mắc phải, rút kinh nghiệm, nếu là vấn đề gì hay ho thì có thể viết thành blog cũng được :))

Nếu fail ở bất kì bước nào trong các bước trên đều dẫn tới thất bại trong việc giải quyết vấn đề.

<div class="bigquote"><i>If I had an hour to solve a problem I'd spend 55 minutes thinking about the problem and 5 minutes thinking about solutions.</i>
<div class="quotecite">— <a href="https://quoteinvestigator.com/2014/05/22/solve/">Albert Einstein</a></div>
</div>

Không đọc kĩ yêu cầu, bạn có thể sẽ giải quyết sai vấn đề. Không hiểu rõ vấn đề, bạn có thể sẽ chẳng bao giờ tìm ra nguyên nhân của nó, đừng nói tới hướng giải quyết. Không phân tích được đâu là nguyên nhân gốc rễ (root cause), giải pháp của bạn có thể sẽ là một giải pháp nửa vời, và tệ hơn là gây ra regression (có bao giờ bạn tự hỏi, tại sao fix bug hoài mà càng fix thì càng ra bug không?). Không có kế hoạch hành động cụ thể, bạn sẽ bị lệch hướng khi thực hiện và tai hại hơn là gây ảnh hưởng đến các thành phần khác của hệ thống. Giải quyết xong vấn đề là vứt nó qua một bên luôn không bao giờ nhìn lại, cứ thế bạn sẽ không bao giờ thu được tí kinh nghiệm gì ngoài việc làm nhiều rồi quen tay.

Một software engineer giỏi không phải là người biết nhiều kĩ thuật hay ngôn ngữ, mà là người có khả năng giải quyết vấn đề tốt.

Và muốn biết một người có khả năng giải quyết vấn đề tốt hay không, thì cách dễ nhất là giao cho họ một vấn đề nào đó để thử. Cũng giống như làm toán, việc giải các bài thuật toán là cơ hội tốt nhất để một người có thể phô diễn khả năng giải quyết vấn đề của mình, thông qua cách mà họ tiếp cận vấn đề, phân tích, giải thích và đưa ra phương hướng giải quyết, cuối cùng, là bước implement, test và đánh giá thuật toán.

Chắc chắn là có rất nhiều cách khác để thử kĩ năng giải quyết vấn đề, nhưng một bài thuật toán đơn giản là quá đủ cho một cuộc phỏng vấn chừng 45 phút.

---

Vậy mới thấy, không chỉ trong phỏng vấn, giải quyết vấn đề luôn là một kĩ năng cơ bản mà bất kỳ ai, dù làm bất kỳ công việc gì đều cần đến. Đối với nghề lập trình, chúng ta thường hay bị cuốn theo những thứ không cần thiết như vấn đề ngôn ngữ, công nghệ mà quên đi những thứ đáng ra rất cần thiết như là problem solving, hay những thứ công cụ không thể thiếu cho công việc giải quyết vấn đề (như là nền tảng kiến thức về computer science).

Và thật đáng buồn khi những bài viết như thế này, đưa ra lời khuyên về rèn luyện kĩ năng thuật toán, kiến thức nền tảng lại ít được like và share hơn những thứ vô bổ, đầy rẫy ngoài kia (như cái động tào lao có nói tới ở đầu bài) :cry: ~đọc tới đây rồi cần làm gì thì tự hiểu đi nha~
