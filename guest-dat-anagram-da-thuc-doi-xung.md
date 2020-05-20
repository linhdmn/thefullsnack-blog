---
title: Anagram và đa thức đối xứng
published: guest
date: 2018-05-16 22:05:15
tags: guest, math, algorithm
description: Một bài viết mang hơi hướng "phức tạp hóa" của bạn Đạt về cách tiếp cận và giải bài toán xác định Anagram...
image:
---

Bài viết của bạn [Tat-Dat Tran](https://github.com/taurandat), Senior Data Scientist đến từ QUONIE, được đăng trong chuyên mục [Khách mời](/tags/guest.html).

Các bạn cũng có thể đọc [bài viết gốc](http://tatd.at/problem-solving/2018-05-13-anagram/) trên blog của tác giả.

---

> **Bài toán**: Cho hai chuỗi $ s $ và $ t $, làm thế nào để kiếm tra được
> chuỗi $ t $ có phải là **[anagram](https://en.wikipedia.org/wiki/Anagram)**
> của chuỗi $ s $ hay không?

Đơn giản nhất là đếm số lần xuất hiện của từng ký tự trong mỗi chuỗi, rồi kiểm
tra xem chúng có khớp nhau không.

```python
def correct_anagram(s, t):
    if len(s) != len(t):
        return False

    count_s = [0] * 256  # there are 256 ASCII characters
    count_t = [0] * 256

    for s_i, t_i in zip(s, t):
        count_s[ord(s_i)] += 1
        count_t[ord(t_i)] += 1

    for i in range(256):
        if(count_s[i] != count_t[i]):
            return False
    return True
```

Nhưng vậy thì còn gì là vui nữa.

---

# Lời giải sai đầu tiên

Thay vì phải kiểm tra mảng đếm, ta thử cheat bằng cách **checksum** các ký tự:

```python
def checksum_anagram(s, t):
    if len(s) != len(t):
        return False

    diff = 0
    for s_i, t_i in zip(s, t):
        a, b = ord(s_i), ord(t_i)
        diff += a - b

    return (diff == 0)
```

Dĩ nhiên là sai rồi. Vấn đề là việc tìm ra được **collison** có khó không. Câu trả
lời là không quá khó. Dễ thấy, **collison** là các cặp chuỗi $ s $ và $ t $
với các ký tự $ s_i $, $ t_i $ tương ứng, sao cho thoả phương trình:

<math>
\sum _{i=1}^{n} s_i = \sum _{i=1}^{n} t_i 
</math>

với $ t_i $ không phải là **hoán vị** của $ s_i $ (nghiệm hiển nhiên).

Xét trường hợp chuỗi s và t chỉ có hai ký tự, một **collison** khả dĩ cho hàm
`checksum_anagram` là **az** và **dw** ($ 97 + 122 = 100 + 119 $).

---

# Một cải tiến mới

Chỉ kiểm tra tổng không thôi thì chưa đủ. Nhận xét thấy nếu hai chuỗi là **anagram**
của nhau, thì số lần xuất hiện của mỗi ký tự trong cả hai chuỗi phải là số chẵn.
Do $ a \oplus a = 0 $, ta có thể hiện thực hoá ý tưởng bên trên khá nhanh với
toán tử **xor**. Hàm `checksum_anagram` trở thành:

```python
def xor_checksum_anagram(s, t):
    if len(s) != len(t):
        return False

    diff = 0
    xor = 0
    for s_i, t_i in zip(s, t):
        a, b = ord(s_i), ord(t_i)
        diff += a - b
        xor = xor ^ a ^ b

    return (diff == 0 and xor == 0)
```

Ở đây, ta đã loại đi được kha khá các trường hợp sai mà hàm vẫn trả về đúng như
**az** và **dw**. Tuy nhiên, phép **xor** thực ra không có ý nghĩa gì, bởi với một
cặp $ (s, t) $ là **collison** của hàm `checksum_anagram`, thì cặp $ (s+s, t+t) $
cũng sẽ là **collison** của hàm `xor_checksum_anagram`, như **azaz** và **dwdw**.

---

# With great power comes great responsibility

Để ý rằng cách kiểm tra tổng ban đầu sẽ luôn đúng, nếu như hai chuỗi $ s $ và
$ t $ chỉ có 1 ký tự (thanks, Captain Obvious). Một ý tưởng khá tự nhiên là:
Thay vì chỉ kiểm tra sai số bậc 1, ta có thể đi thêm bước nữa với việc kiểm tra
sai số các bậc cao hơn (**great power**).

Bắt đầu bằng sai số bậc 2:

```python
def squared_checksum_anagram(s, t):
    if len(s) != len(t):
        return False

    diff_1, diff_2 = 0
    for s_i, t_i in zip(s, t):
        a, b = ord(s_i), ord(t_i)
        diff_1 += a - b
        diff_2 += a**2 - b**2

    return (diff_1 == 0 and diff_2 == 0)
```

Kiểm tra bruteforce thấy cách này là đúng với các chuỗi có 2 ký tự.

```python
def generate(upper, lower, true_method, mock_method):
    chars = "abcdefghijklmnopqrstuvwxyz"

    for i in range(upper, lower+1):
        all_strings = [''.join(x) for x in itertools.product(chars, repeat=i)]
        print("Length = %d. Considering %d possibilies..." % (i, len(all_strings) ** 2))
        for s in all_strings:
            for t in all_strings:
                if true_method(s, t) != mock_method(s, t):
                    print("Counter example found:", s, t)
                    return

generate(1, 2, correct_anagram, squared_checksum_anagram)
```

Một ý tưởng tự nhiên khác chợt đến là: Nếu độ dài của hai chuỗi là $ n $, thì
việc kiểm tra sai số đến bậc thứ $ n $ sẽ đủ tốt để kết quả mà nó trả về là luôn
đúng (**great responsibilities**).

```python
def powered_checksum_anagram(s, t):
    if len(s) != len(t):
        return False

    n = len(s)
    diffs = [0] * n
    for s_i, t_i in zip(s, t):
        a, b = ord(s_i), ord(t_i)
        for i in range(n):
            diffs[i] = a**(i+1) - b**(i+1)

    for diff in diffs:
        if diff != 0:
            return False
    return True
```

Giờ thì tìm cách chứng minh thôi.

---

# Chứng minh

Theo phương pháp trên, một **collison** sẽ tồn tại khi và chỉ khi hệ phương trình
sau không tồn tại nghiệm nguyên:

<math>
\begin{align}
\sum _{i=1}^{n} s_i &= \sum _{i=1}^{n} t_i  \\
\sum _{i=1}^{n} {s_i}^2 &= \sum _{i=1}^{n} {t_i}^2  \\
...  \\
\sum _{i=1}^{n} {s_i}^n &= \sum _{i=1}^{n} {t_i}^n \\
\end{align}
</math>

ngoại trừ nghiệm hiển nhiên với $ s_i $ là hoán vị của $ t_i $.

Đặt:

<math>
S_k = \sum _{i=1}^{n} {s_i}^k  \\
T_k = \sum _{i=1}^{n} {t_i}^k 
</math>

<math>
A_1 = \sum _{i=1}^{n} s_i, A_2 = \sum _{i \neq j} {s_i s_j}, A_3 = \sum _{i \neq j \neq k \neq i} {s_i s_j s_k}, ..., A_n = s_1 s_2 s_3 ... s_n  \\
B_1 = \sum _{i=1}^{n} t_i, B_2 = \sum _{i \neq j} {t_i t_j}, B_3 = \sum _{i \neq j \neq k \neq i} {t_i t_j t_k}, ..., B_n = t_1 t_2 t_3 ... t_n 
</math>

Hệ ban đầu trở thành $ S_k = T_k $ với mọi $ k $.

Ta có:

<math>
 A_1 = S_1 = T_1 = B_1 \\
 2 A_2 = A_1 S_1 - S_2 = B_1 T_1 - T_2 = 2 B_2 \\
 3 A_3 = A_2 S_1 - A_1 S_2 + S_3 = B_2 T_1 - B_1 T_2 + T_3 = 3 B_3 \\
 ...
 </math>

Hay tổng quát hơn:

<math>
 k A_k = \sum _{i=1}^{k} (-1)^{i-1} A_{k-i} S_i \\
 k B_k = \sum _{i=1}^{k} (-1)^{i-1} B_{k-i} T_i 
</math>

Đây là nội dung của [đẳng thức Newton](https://en.wikipedia.org/wiki/Newton%27s_identities).

Do $ A_k, B_k $ chỉ được tính bởi $ A_m, B_m $ với $ m < k $ nên $ A_i = B_i $
với mọi $ 1 \leq i \leq n $.

Gọi $ S(x) $ và $ T(x) $ là đa thức có các nghiệm lần lượt là $ s_i $ và
$ t_i $. Nói cách khác:

<math>
S(x) = (x - s_1) (x - s_2) ... (x - s_n) = \prod _{i=1}^n (x - s_i) \\
T(x) = (x - t_1) (x - t_2) ... (x - t_n) = \prod _{i=1}^n (x - t_i) 
</math>

Khai triển lại $ S(x) $ và $ T(x) $ trở thành:

<math>
S(x) = x^n + A_1 x^{n-1} + A_2 x^{n-2} + ... A_{n-1} x + A_n  \\
T(x) = x^n + B_1 x^{n-1} + B_2 x^{n-2} + ... B_{n-1} x + B_n 
</math>

Do $ A_i = B_i $ với mọi $ 1 \leq i \leq n $, nên hai đa thức $ S(x) $ và
$ T(x) $ thực chất là một, và bởi vậy nên chúng có chung tập nghiệm. Nói cách
khác, $ s_i $ là hoán vị của $ t_i $.

Trên màn hình xuất hiện bảng thông báo:

> Xin chúc mừng! Từ một bài toán đơn giản thuộc cấp độ `easy` trên
> [LeetCode](https://leetcode.com/problems/valid-anagram/), bạn đã biến nó trở
> nên cực kỳ phức tạp, chỉ có thể chứng minh tính đúng đắn bằng
> [dao mổ trâu](https://en.wikipedia.org/wiki/Power_sum_symmetric_polynomial),
> và cũng tăng luôn cả độ phức tạp thuật toán từ $ O(n) $ sang $ O(n^2) $!
