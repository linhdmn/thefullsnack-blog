---
title: Tăng tốc tính toán bằng cách chuyển hệ cơ số
published: true
date: 2018-02-19 19:23:06
tags: algorithm, math
description: 
image: 
---
Cách đây mấy hôm mình có đọc trên một nhóm học thuật toán của mấy bạn chuyên tin bàn về chuyện dùng phương pháp _nhân nhóm số_ để tính toán hiệu quả hơn với các số lớn, đọc không hiểu mô tê gì vì mấy bạn ấy chỉ share code :sob:

Chạy về hỏi mấy ông semen (í nhầm, senpai) trong nhóm [ruby-vietnam](https://github.com/ruby-vietnam/hardcore-rule/tree/master/algorithms) thì đc suggest nhiều cách khác nhau, nhưng nói chung là đổi về hệ cơ số khác lớn hơn hệ thập phân.

Trước khi bắt đầu, mình xin lưu ý, đây chỉ là một bài toán lớp 2, không hơn không kém, có thể hồi đi học bạn rất chăm học nên đọc vô đã nhớ ngay, riêng mình hồi đó học ko giỏi lắm nên quên hết, cho nên bây giờ càng viết càng thấy nó mới và lạ :joy:. Hãy độ lượng với kẻ quên toán lớp 2 này, và nhiệt tình thảo luận ở phần comment cuối bài nếu phát hiện ra mình nói nhăng nói cuội gì trong bài nhé :wink:.

## Phương pháp tính

Giả sử ta muốn nhân hai số `A = 2561` và `B = 3123`.

Mình tạm kết luận là không có ai quên cách nhân chia đã học từ hồi lớp 2, vậy thì chúng ta sẽ tiếp tục với kết quả là:

<math>
\begin{align}
& 2561 \\
\times \ & \underline{3123} \\
& 7683 \\
5&122 \\
25&61 \\
768&3 \\
\overline{799}&\overline{8003}
\end{align}
</math>

Với cách tính bằng tay như trên, việc ghi số và "nhớ" số khi nhân một cột, được thực hiện trên nguyên tắc, số ghi vào sẽ là kết quả của phép chia lấy dư `n mod 10`, số được "nhớ" sẽ là phần nguyên của phép chia `n / 10`.

Giới hạn cho từng giá trị ở đây không lớn hơn 10, cũng chính là số chữ số có trong hệ cơ số mà chúng ta đang sử dụng (hệ thập phân - decimal).

Bây giờ chúng ta thử chuyển sang dùng hệ cơ số khác, lớn hơn hệ thập phân, mà cụ thể ở đây mình sẽ dùng hệ cơ số 100 (`b = 100`).

Mỗi số `A` và `B` được biểu diễn trong hệ thập phân bằng 4 chữ số (4 words), lần lượt là `(2, 5, 6, 1)` và `(3, 1, 2, 3)`, còn trong hệ 100 thì được biểu diễn bằng một cặp số (2 words), lần lượt là `(25, 61)` và `(31, 23)`.

Cách thực hiện phép nhân trên hệ cơ số 100 không khác gì so với hệ thập phân, nhưng lưu ý phần giới hạn của mỗi cột khi thực hiện phép nhân không lớn hơn 100, nên giá trị được ghi vào mỗi cột phải là `n mod 100`, và phần số được "nhớ" sẽ là phần nguyên của phép chia `n / 100`.

<math>
\begin{align}
& 25 \ 61 \\
\times \ & \underline{31 \ 23} \\
\phantom{0}5 \ & 89 \ 03 \\
\phantom{0}7 \ 93 \ & 91 \\
\overline{7\ 99\ }& \overline{80 \ 03}
\end{align}
</math>

Có thể thấy, trên hệ thập phân, chúng ta cần tới 16 phép nhân trước khi tìm ra được kết quả, còn với hệ 100, chỉ cần 4 phép nhân [^1].

Trên thực tế, nếu lựa chọn hệ cơ số càng lớn, thì tổng số phép nhân cần dùng càng ít. Tuy nhiên khi implement, bạn cũng cần phải cân nhắc đến yếu tố giới hạn của kiểu dữ liệu mà mình đang sử dụng nữa.

## Chọn hệ cơ số

Vậy câu hỏi đặt ra là nên chọn hệ cơ số bao nhiêu để vừa tăng được tốc độ tính toán, vừa không vượt quá phạm vi cho phép của các kiểu dữ liệu?

Lưu ý là dù ta chuyển hệ cơ số của các số cần nhân, phép nhân giữa các chữ số (words) mà chúng ta thực hiện trong mỗi bước tính toán vẫn là phép nhân hai số trong hệ thập phân.

Một số $( a_{n} \cdots a_0 )$ có n chữ số sẽ được biểu diễn trong hệ thập phân dưới dạng:

<math>
( a_n \cdots a_0 ) = a_n \times 10^n + \cdots + a_0 \times 10^0
</math>

Nếu ta nhân hai số có n chữ số, thì ta có:

<math>
\begin{align}
( a_n \cdots a_0 ) \times ( b_n \cdots b_0 ) & = ( a_n \times 10^n + \cdots + a_0 \times 10^0 ) \times ( b_n \times 10^n + \cdots + b_0 \times 10^0 ) \\
& = a_n \times b_n \times 10^{2n} + \cdots + a_0 \times b_0 \times 10^0
\end{align}
</math>

Như vậy kết quả của phép nhân sẽ là một số có ít nhất 2n chữ số, hay nói cách khác, nếu hai words có kích thước n chữ số trong hệ cơ số $10^n$ nhân với nhau, thi khả năng nó sẽ tạo thành một word mới có kích thước gấp đôi kích thước của mỗi word ban đầu.

Và trên máy tính, việc thay đổi kích thước word này quyết định việc chúng ta phải chọn kiểu dữ liệu nào cho các word.

Vậy thì khi lựa chọn hệ cơ số thích hợp cho phép nhân, ta cân nhắc các yếu tố sau:

1. Hệ cơ số càng lớn thì số phép tính cần dùng sẽ càng ít, tính toán sẽ nhanh hơn
2. Việc chuyển đổi từ hệ cơ số này về hệ thập phân phải ít phức tạp nhất có thể
2. Kết quả của phép nhân hai số trên hệ cơ số này phải đảm bảo nằm trong phạm vi xử lý của máy tính.

Trong số các kiểu dữ liệu mà chúng ta có thể dùng trong việc tính toán số nguyên, thì có hai kiểu khả dụng trong trường hợp này là `Int 32-bit` và `Int 64-bit` [^2]. Và tất nhiên ta sẽ chọn 64-bit [^3] cho kết quả tính toán.

Kiểu số nguyên 64-bit có giá trị lớn nhất là `9,223,372,036,854,775,807`, tức là nằm trong phạm vi $10^{18} \leq \texttt{MAX_I64} \leq 10^{19}$, như vậy con số 2n của chúng ta sẽ là 18, vậy hệ cơ số lớn nhất mà chúng ta có thể chọn ở đây là $b = 10^9$.

## Thuật toán và Implementation

Thuật toán chuyển đổi một giá trị từ hệ thập phân sang một hệ cơ số $10^n$ có thể được mô tả như sau:

<div class="box-green skip" style="padding-left: 10px; padding-right: 10px">
<p><b>Input:</b> chuỗi s chứa số cần chuyển, và số n, là số mũ của hệ cơ số $10^n$</p>
<p>
<b>1:</b> Khởi tạo mảng r chứa dãy số kết quả</br>
<b>2:</b> Chừng nào chuỗi s chưa rỗng:</br>
<b>3:</b> &emsp;Lấy n kí tự tính từ cuối chuỗi s, chuyển thành một số m</br>
<b>4:</b> &emsp;Chèn số m vào mảng r</br>
<b>5:</b> &emsp;Cắt bỏ phần kí tự đã chuyển đổi thành m trong s
</p>
<p><b>Output:</b> Mảng r biểu diễn số s trong hệ cơ số $10^n$</p>
</div>

Implement bằng Rust:

```rust
fn base_conv(a: &str, n: usize) -> Vec<u64> {
    let mut result = vec![];
    let mut s = a.to_owned();
    let mut len = s.len();
    while len > 0 {
        let mut pos = 0;
        if n <= len {
            pos = len - n;
        }
        let n = s[pos..].to_string().parse::<u64>().unwrap();
        result.push(n);
        s = s[..pos].to_string();
        len = s.len();
    }
    return result;
}
```

Đối với thuật toán trên, các thành phần của một số sau khi chuyển đổi sẽ có thứ tự ngược so với số đầu vào, chúng ta không cần thay đổi gì vì trước sau gì chúng ta cũng sẽ thực hiện phép nhân theo thứ tự như vậy luôn.

```rust
1234 -> [34, 21]
```

Thuật toán nhân hai số sau khi chuyển đổi hệ cơ số được mô tả như sau:

<div class="box-green skip" style="padding-left: 10px; padding-right: 10px">
<p><b>Input:</b> Giá trị n và hai mảng a, b chứa các bộ số biểu diễn hai số cần nhân ở hệ cơ số $10^n$</p>
<p>
<b>1:</b> Khởi tạo mảng r chứa kết quả, kích thước bằng tổng độ dài hai mảng a và b</br>
<b>2:</b> Duyệt qua từng phần tử thứ i của mảng b:</br>
<b>3:</b> &emsp;Duyệt qua từng phần tử thứ j của mảng a:</br>
<b>4:</b> &emsp;&emsp;Thực hiện phép nhân hai số a[j] và b[i] tương ứng cho từng cột của r</br>
<b>5:</b> &emsp;&emsp;Nếu giá trị của phép nhân lớn hơn $10^n$ thì thực hiện tách số dư giống khi nhân bằng tay để cộng vào cột kế tiếp</br>
</p>
<p><b>Output:</b> Mảng r biểu diễn kết quả nhân hai số a và b trong hệ cơ số $10^n$</p>
</div>

Implementation:

```rust
fn multiply_base(a: Vec<u64>, b: Vec<u64>, n: u32) -> Vec<u64> {
    let mut result = vec![];
    result.resize(a.len() + b.len(), 0u64);
    let mut bot = 0; let mut up = 0;
    let mut flag = 0u64; let base = 10u64.pow(n);
    for i in 0..b.len() {
        for j in 0..a.len() {
            let mut t = (a[j] * b[i]) as u64 + flag + result[up];
            flag = 0;
            if t >= base {
                flag = t / base;
                t = t % base;
            }
            result[up] = t;
            if j >= a.len()-1 {
                result[up+1] = flag;
                flag = 0;
            }
            up += 1;
        }
        bot += 1;
        up = bot;
    }
    result
}
```

Và cuối cùng là chuyển đổi kết quả về dạng số được biểu diễn trong hệ thập phân. Không có gì đáng nói trong hàm này ngoại trừ trong một số trường hợp, mảng kết quả sẽ có dạng `[2, 13]` nhưng thực chất lại biểu diễn giá trị `1302`, số `0` ở đầu đương nhiên bị bỏ đi, ta xử lý các trường hợp này bằng cách chèn thêm các số `0` này cho đủ.

```rust
fn convert_back(num: Vec<u64>, base: usize) -> String {
    let mut result = String::new();
    let filtered: Vec<u64> = num.into_iter().filter(|x| *x > 0u64).collect();
    for i in 0..filtered.len() {
        let n = filtered[i];
        if n != 0 {
            if i >= filtered.len()-1 {
                result = format!("{}{}", n, result);
            } else {
                result = format!("{}{}", format!("{num:>0width$}", num=n, width=base), result);
            }
        }
    }
    result
}
```

Đến đây thì chúng ta đã hoàn thành rồi. Bạn có thể tham khảo chương trình đầy đủ viết bằng Rust [tại đây](https://gist.github.com/huytd/6d5ba5926b56bfba0ab1f80cb106d695) hoặc chạy trực tiếp [tại đây](https://play.rust-lang.org/?gist=50e4da80a076623ba5de3b04410c1739&version=stable).

Chúng ta có thể thử thực hiện phép nhân hai số bự bự kiểu như:

<math>
29123841234812351239412 \times 3496123842341123491234123
</math>

Kết quả sẽ là:

<math>
101820555721585007964354044327028370336516855676
</math>

Ở trên chúng ta thực hiện phép nhân một số dài 23 chữ số với một số dài 25 chữ số, tất nhiên là đều vượt ra khỏi phạm vi tính toán trực tiếp của kiểu số nguyên lớn nhất mà các CPU thông thường có thể xử lý được (64-bit). Nếu chúng ta dùng phương pháp nhân từng chữ số giống với cách tính toán trên giấy, chúng ta sẽ cần `23 * 25 = 575` phép nhân trước khi ra được kết quả.

Bằng kĩ thuật chuyển đổi hệ cơ số, mà cụ thể là cơ số $10^9$, chúng ta đã tách hai số đầu vào thành 2 bộ số chỉ gồm 3 words cho mỗi bộ:

<math>
(29123, 841234812, 351239412) \times (3496123, 842341123, 491234123)
</math>

Và chỉ cần dùng `3 * 3 = 9` phép nhân để có được kết quả. Độ phức tạp của thuật toán giảm đi (xấp xỉ) 64 lần. Một sự tối ưu không hề nhẹ.

Tối ưu hóa các phép tính là một đề tài khá là thú vị, thậm chí đến bây giờ, việc tìm ra thuật toán nhanh nhất để nhân hai số vẫn còn là một câu hỏi chưa có lời giải đáp của ngành khoa học máy tính [^4]. Nếu quan tâm tới chủ đề này, Wikipedia có lẽ là điểm bắt đầu để các bạn có thể tìm hiểu thêm về các thuật toán nhân hiện có [^5].

Riêng cá nhân mình giờ chỉ có một sự quan tâm duy nhất: đến bao giờ thi tài khoản Coinbase, Poloniex và ngân hàng của mình mới có cơ hội để xử lý với những con số lớn đến như thế? :joy:

Lời cuối, xin chân thành cảm ơn các anh [@linxGnu](https://github.com/linxGnu), [@unrealhoang](https://github.com/unrealhoang) cùng nhiều anh em khác trong nhóm [ruby-vietnam/algorithms](https://github.com/ruby-vietnam/hardcore-rule/tree/master/algorithms) đã tích cực khai sáng, thảo luận và hỗ trợ mình hoàn thành bài viết này.

---

**Notes**

[^1]: Nếu bạn implement một thuật toán thực hiện nhân hai số lớn bằng phương pháp tính tay như trên, thì mỗi một phép nhân chỉ tính là một instruction, không cần biết hai số được nhân có bao nhiêu chữ số.

[^2]: Vậy tại sao không chọn các kiểu dữ liệu lớn hơn nữa, ví dụ như `Int 128-bit`? hay các kiểu không phải số nguyên? Lý do là vì số nguyên là kiểu dữ liệu có thể tính toán hiệu quả nhất trên máy tính, và hiện tại vẫn chưa có nhiều CPU natively support các kiểu số nguyên lớn hơn 64-bit.

[^3]: Trừ JavaScript ra, vì chỉ có thể xử lý các số nguyên có giá trị lớn nhất là 53-bit. Xem thêm: http://speakingjs.com/es5/ch11.html#_ranges_of_integers 

[^4]: List of unsolved problems in computer science, Wikipedia (https://en.wikipedia.org/wiki/List_of_unsolved_problems_in_computer_science#Other_algorithmic_problems)

[^5]: Multiplication algorithm, Wikipedia (https://en.wikipedia.org/wiki/Multiplication_algorithm)
