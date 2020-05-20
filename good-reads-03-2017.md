---
title: Weekend Reading - 03/2017
published: true
date: 2017-03-19 13:24:29
tags: random, algorithm, review, math, rust
description:
image:
---
Bài viết này tổng hợp lại một vài vấn đề hay ho mà mình đọc được trong cuối tuần này. Từ giờ mình sẽ cố gắng note lại những nội dung mình đọc được trong những cái weekend như này, một phần để cho khỏi bị quên, một phần hy vọng có thể chia sẽ được cho những ai quan tâm.

## [A Tutorial Introduction to the Lambda Calculus](https://arxiv.org/abs/1503.09060)

Paper này được giới thiệu là một "painless introduction về $\lambda$-calculus". Trước khi đọc thì cũng nghe nhiều ae nói qua về lambda calculus nhưng vẫn chưa hình dung ra được nó là gì và tại sao nó lại khá hay ho.

Lamda calculus là "ngôn ngữ lập trình" dựa trên việc tạo ra các annonymous function, có dạng:

<math>
\lambda x.x
</math>

Trong đó $\lambda$ là ký hiệu định nghĩa hàm, kí hiệu $x$ đầu tiên giúp xác định biến truyền vào hàm và kí hiệu $x$ thứ hai là phần thân hàm.

Ví dụ rõ ràng hơn, ta có thể viết lại hàm số $f(x) = x^2$ dưới dạng cú pháp của lambda calculus như sau:

<math>
f = \lambda x.x^2
</math>

Thao tác "sử dụng" hàm này được gọi là _apply_, ví dụ:

<math>
f(7) = (\lambda x.x^2) 7 = 7^2 = 49
</math>

Ở ví dụ trên ta _apply_ giá trị số 7 vào hàm $\lambda x.x^2$ bằng cách thay thế toàn bộ kí hiệu $x$ ở phần thân hàm thành số 7.

Các tài liệu liên quan (có cả những cái mình chưa đọc :)) note lại để khi nào có thời gian lại lôi ra đọc)

- [A Tutorial Introduction to the Lambda Calculus](https://arxiv.org/abs/1503.09060), Raul Rojas, 2015.
- [Lambda Calculus](http://wiki.c2.com/?LambdaCalculus), Ward Cunningham, 2013.
- [Introduction to Lambda Calculus](http://www.cse.chalmers.se/research/group/logic/TypesSS05/Extra/geuvers.pdf), Henk Barendregt & Erik Barendsen, 2000.
- [Lambda Calculus](https://brilliant.org/wiki/lambda-calculus/), Brilliant.org, 2017.
- [Lambda Calculus Implement in Haskell](http://dev.stephendiehl.com/fun/lambda_calculus.html), Stephen Diehl

## [Regular Expression Matching with a Trigram Index](https://swtch.com/~rsc/regexp/regexp4.html)

Bài này nói về cơ chế tìm kiếm sử dụng regex trên một lương dữ liệu cực kì lớn (ví dụ source code của toàn bộ các project open source trên toàn thế giới) bằng cách build một bộ _n_-grams index dựa trên toàn bộ nội dung đống source đó, mà ở đây bài báo chọn _3_-grams.

Ví dụ có 3 document sau:

```
(1) Google Code Search
(2) Google Code Project Hosting
(3) Google Web Search
```

Bộ trigram index sẽ có dạng:

```
_Co: {1, 2}     Sea: {1, 3}     e_W: {3}        ogl: {1, 2, 3}
_Ho: {2}        Web: {3}        ear: {1, 3}     oje: {2}
_Pr: {2}        arc: {1, 3}     eb_: {3}        oog: {1, 2, 3}
_Se: {1, 3}     b_S: {3}        ect: {2}        ost: {2}
_We: {3}        ct_: {2}        gle: {1, 2, 3}  rch: {1, 3}
Cod: {1, 2}     de_: {1, 2}     ing: {2}        roj: {2}
Goo: {1, 2, 3}  e_C: {1, 2}     jec: {2}        sti: {2}
Hos: {2}        e_P: {2}        le_: {1, 2, 3}  t_H: {2}
Pro: {2}        e_S: {1}        ode: {1, 1}     tin: {2}
```

Là một bảng tham chiếu dựa trên từng cụm substring 3 kí tự của mỗi từ trong từng document, giá trị tham chiếu là số thứ tự của tài liệu có chứa trigram đó.

Khi tìm kiếm thì chúng ta sẽ build một query sử dụng các phép _AND_ hoặc _OR_ để tìm kiếm, ví dụ với input là `/Google.*Search/`, ta build được query sau:

```
Goo AND oog AND ogl AND gle AND Sea AND ear AND arc AND rch
```

Kết quả tìm kiếm sẽ là kết quả thu được từ việc thực hiện các phép `AND` trên các tập hợp tương ứng ở index.

Khi đọc title thì mình mang một cái thắc mắc là: Tại sao lại là 3 mà không phải là một con số khác? Tác giả trả lời trong bài: vì 2 là quá ít, 4 thì lại quá nhiều, nên cứ lấy số 3 thôi...

## [Method Syntax: self, &self và &mut self trong Rust](https://doc.rust-lang.org/book/method-syntax.html)

Trong Rust, khi implement một method cho một `struct` với từ khóa `impl`, chúng ta có 3 chỉ định tham số `self` (tương tự như `this` trong các ngôn ngữ khác) vào cho một method là: 

- `self`: khi chúng ta cần sử dụng `self` bên trong hàm như là giá trị của chính nó trên stack (take luôn ownership).
- `&self`: khi chúng ta cần `self` là một reference (borrow thôi, không take ownership).
- `&mut self`: khi chúng ta cần `self` là một mutable reference.

Và mặc định thì chúng ta nên sử dụng `&self`.

## [Applications of Tree, Discrete Mathematics and Its Applications](https://www.amazon.com/Discrete-Mathematics-Its-Applications-Seventh/dp/0073383090/ref=pd_sbs_14_t_0?_encoding=UTF8&psc=1&refRID=1TCGJV0XWWWJ9QSHSNNH)

Vì đang làm một ứng dụng liên quan đến kiểu dữ liệu cây (Tree) nên mình cũng muốn tìm hiểu bài bản về nó một tí, nên đọc qua chương này trong cuốn _Discrete Mathematics and Its Applications_.

Mặc dù đã đọc một vài phiên bản tiếng Việt, dịch ra từ cuốn này như cuốn _"Toán rời rạc ứng dụng trong tin học", NXB Giáo Dục_ hay các cuốn có nội dung tương tự như _"Thuật toán trong Máy tính", NXB Thống Kê_, nhưng mình thật sự bất ngờ vì lượng kiến thức mà bản gốc này cung cấp, bên cạnh việc trình bày các khái niệm lý thuyết rất khoa học, cuốn sách còn đưa thêm rất nhiều ví dụ cụ thể, từ đơn giản đến phức tạp. 

Quay trở lại vụ Tree, phần _"Applications of Tree"_ trong cuốn sách có đưa ra các ví dụ minh họa như xây dựng cây tìm kiếm nhị phân cho một tập hợp các chuỗi (string) dựa trên thứ tự alphabet, sử dụng cây quyết định (decision tree) để giải bài toán cân 8 đồng xu, giới thiệu thuật toán Huffman coding, cây trò chơi,... một cách cực kì chi tiết và dễ hiểu.

## Koudan: Miyamoto Musashi

Không tìm thấy link sách gốc, tuy nhiên bản dịch tiếng Việt thì rất nhiều trên mạng, các bạn có thể tìm đọc. 

Koudan (講談) là lối kể chuyện truyền thống của Nhật Bản dưới hình thức diễn thuyết, diễn giả ngồi trên một bục cao và sử dụng giọng đọc truyền cảm của mình để kể lại các giai thoại lịch sử, thường là những mẫu truyện có thêm bớt vài phần hư cấu và chỉ dùng các yếu tố lịch sử để làm bối cảnh, tình tiết cho câu chuyện.

Bản dịch _"Koudan: Miyamoto Musashi"_ là phiên bản "viết" được nhà xuất bản Kodansha đóng lại thành sách từ bản koudan của diễn giả Ito Ryocho thời kì trước Thế chiến thứ II.

Quyển sách kể về cuộc đời của Miyamoto Musashi, một nhân vật lịch sử có thật và cực kì nổi tiếng, được đông đảo người Việt nam biết đến qua các ấn phẩm văn hóa Nhật như tiểu thuyết, truyện tranh, phim ảnh,... 

Mặc dù người dịch đã khuyến cáo là thể loại Koudan rất khó để dịch ra thứ ngôn ngữ khác, vì nó sử dụng các âm điệu, vần, từ ngữ cổ của Nhật Bản, dịch ra sẽ mất hầu hết những cái tinh hoa đặc sắc của Koudan, tuy nhiên mình thấy đây là một bản dịch không tồi, thật tình là rất cảm phục người dịch giả, bỏ công sức ra dịch miễn phí nhưng từng câu từng chữ đều được viết ra một cách rất cẩn thận và có đầu tư, không như vô số các dịch giả khác, dịch xong nhiều khi phá hỏng luôn cả bộ truyện :D 
