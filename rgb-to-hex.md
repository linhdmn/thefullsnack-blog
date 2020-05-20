---
title: Tính nhẩm đổi màu RGB ra Hexadecimal và ngược lại
published: true
date: 2017-07-10 13:33:18
tags: math, code
description: 
image:
---
Mình có một anh bạn người Pháp tên là Aurelien, anh này có một biệt tài đó là **convert được màu RGB sang mã Hex chỉ bằng cách tính nhẩm**.

Phương pháp để tính nhẩm rất đơn giản.

Một màu được biểu diễn ở dạng hexadecimal với một bộ 3 cặp số $RR GG BB$ và nó sẽ tương đương với 3 giá trị trong mã màu $(R, G, B)$.

Như vậy ta có thể tham chiếu mỗi cặp số hexa sang hệ quy chiếu từ 0 đến 255:

<math>
\begin{align}
00 \rightarrow 0  \\
FF \rightarrow 255
\end{align}
</math>

Khi thực hiện chuyển đổi, ta sẽ chuyển từng giá trị $R$, $G$ và $B$ tương ứng.

## Từ RGB sang Hexadecimal

Với một số thập phân $N$ bất kì, ta chuyển nó về dạng một cặp số hexa $H_1 H_2$ bằng cách:

<math>
\begin{align}
H_1 & = N \text{ div } 16 \\
H_2 & = N \text{ mod } 16
\end{align}
</math>

Giả sử với số $231$, ta có thể tính:

<math>
\begin{align}
H_1 & = 231 \text{ div } 16 = 14 \approx E \\
H_2 & = 231 \text{ mod } 16 = 7
\end{align}
</math>

Như vậy, dạng hexadecimal của $231$ sẽ là $E7$.

Thực hiện thao tác trên cho mỗi số $R$, $G$, $B$ ta có thể tìm được mã màu hexa tương ứng.

Ví dụ:

<math>
( 255, 231, 42 ) \rightarrow \#FFE72A
</math>

## Từ Hexadecimal sang RGB

Để tính ngược lại từ 1 cặp số hexa $H_1 H_2$ sang số thập phân $N$, ta có thể tính:

<math>
N = H_1 \times 16 + H_2
</math>

Ví dụ với số $8C$, với $C = 12$, ta có:

<math>
N = 8 \times 16 + 12 = 140
</math>

Hoặc một ví dụ khác, khi đọc code, mình thấy có ai đó trong team define một biến thế này:

```scss
$color_trout: #4a4e5c;
```

Không biết `truất` là màu gì, thì có thể tính nhẩm được bằng cách:

<math>
\begin{align}
4a & = 4 \times 16 + 10 = 74 \\
4e & = 4 \times 16 + 14 = 78 \\
5c & = 5 \times 16 + 12 = 92
\end{align}
</math>

Vậy suy ra `#4a4e5c` là $(74, 78, 92)$, và chiếu theo thang $0 \rightarrow 255$, theo luật phối màu, các giá trị $R = 74$ và $G = 78$ nằm gần đáy, giá trị $B = 92$ nhỉnh hơn 1 tí, ta có thể kết luận đây là một màu xám đậm hơi ngả sang màu xanh.

## Tính nhanh với phép chia cho 16

Để tính nhanh phép chia $a \div b$, ta tách $a$ thành tổng của 2 số nhỏ hơn $a = a_1 + a_2$, chia lần lượt 2 số đó cho $b$, lấy 2 giá trị tìm được cộng lại với nhau ta được kết quả phép chia ban đầu.

<math>
\begin{align}
a & = a_1 + a_2 \\
c & = a \div b = \displaystyle\frac{a_1}{b} + \displaystyle\frac{a_2}{b}
\end{align}
</math>

Để chia nhanh cho 16, thì một trong 2 số $a_1$ và $a_2$ nên là bội số lớn nhất của 16 mà ta có thể tìm được.

Nói ra dài dòng, ví dụ cho dễ hiểu, với phép chia $231 \div 16$, ta có thể tách $231 = 128 + 103$. Từ đó ta có:

<math>
\begin{align}
c_1 & = 128 \div 16 = 8 \\
c_2 & = 103 \div 16 = 6 \\
c & = c_1 + c_2  = 8 + 6 = 14
\end{align}
</math>

Nếu thấy số $103$ vẫn còn khó để tính nhẩm quá thì ta có thể chia nhỏ nó ra tiếp, và chỉ cần lấy phần nguyên tìm được. Ví dụ $103 = 96 + 7$. Phần dư tìm được cũng chính là giá trị $H_2$.

## Kết luận

Vậy kĩ năng này có cool không? Quá cool luôn. Trong khi thằng bên cạnh lọ mọ copy mã màu ra để tra Google xem nó màu gì thì bạn dễ dàng tính ngay ra giấy và đoán được đặc điểm/màu sắc cần tìm.

Chỉ một vài phép tính đơn giản nhưng đem lại cho bạn rất nhiều lợi ích:

- Khả năng tính nhẩm tăng lên
- Kĩ năng phối màu tăng lên
- Trông bạn cũng ngầu hơn, đâu đó sẽ có người nhìn bạn với ánh mắt nể phục, là trai hay gái thì còn tùy :))
