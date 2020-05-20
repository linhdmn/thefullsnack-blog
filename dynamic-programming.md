---
title: Phân tích và tiếp cận bài toán Quy hoạch động
published: true
date: 2017-12-27 15:04:11
tags: algorithm
description: 
image:
---
Với sự hỗ trợ của anh [@linxGnu](https://github.com/linxGnu) (là dân competitive programming thứ thiệt [^1]) cùng với các anh em trong nhóm `#algorithm` [^2] của [Cộng đồng Ruby Việt Nam](http://chat.ruby.org.vn), mình (là dân copypáste programming thứ thiệt) tổng hợp lại qua các trao đổi, thảo luận, nhằm mục đích giới thiệu với bạn đọc các dạng thuật toán thường gặp và cách phân tích, tiếp cận các dạng bài đó.

---

Bài đầu tiên của series, chúng ta nghiên cứu về một kĩ thuật khá phổ biến và cũng khá là "xương", đó là phương pháp **Quy hoạch động** (Dynamic Programming), thông qua một bài tập trên Leetcode.

Đó là bài **Delete and Earn**, tại địa chỉ https://leetcode.com/problems/delete-and-earn/description/

## Problem

Cho một mảng số nguyên $\texttt{nums}$, bạn có thể thực hiện những thao tác sau đây trên mảng:

- Chọn một số $\texttt{nums[i]}$ bất kỳ thì để kiếm được $\texttt{nums[i]}$ điểm.
- Đồng thời cũng phải xóa đi **tất cả** các giá trị $\texttt{nums[i] - 1}$ hoặc $\texttt{nums[i] + 1}$ ngay sau đó.

Ban đầu bạn có 0 điểm, cho biết số điểm cao nhất mà bạn có thể kiếm được.

**Ví dụ 1:**

<pre>
Input: nums = [3, 4, 2]
Output: 6

Chọn 4 để kiếm 4 điểm, như vậy 3 sẽ bị xóa.
Tiếp theo, chọn 2 để kiếm 2 điểm. Như vậy tổng cộng kiếm được 6 điểm.
</pre>

**Ví dụ 2:**

<pre>
Input: nums = [2, 2, 3, 3, 3, 4]
Output: 9

Đầu tiên, chọn 3 để kiếm được 9 điểm, như vậy tất cả các phần tử có giá trị
2 và 4 đều sẽ bị xóa.
</pre>

**Lưu ý:**
- Độ dài tối đa của $\texttt{nums}$ là $20000$.
- Mỗi phần tử $\texttt{nums[i]}$ là một số nguyên trong phạm vi $[1, 10000]$.

## Solution

Ta thấy, mỗi khi chọn tất cả các phần tử có giá trị $\texttt{nums[i]}$ để cộng vào tổng điểm, thì ta không thể chọn các phần tử có giá trị $\texttt{nums[i] + 1}$ hay $\texttt{nums[i] - 1}$ được nữa, nên phải xóa nó đi (cái này hay gọi là _ăn không được thì đạp đổ_ đấy).

Đề bài bảo tìm giá trị lớn nhất có thể có của tổng điểm, tức là dạng tìm giá trị tối ưu, theo như kinh nghiệm trận mạc, ta có thể giải nó bằng phương pháp Quy hoạch động.

Đối với một giá trị $x$ bất kỳ, ta có $F(x)$ là tổng điểm lớn nhất mà ta có thể thu được, có hai trường hợp có thể xảy ra: 

- **Trường hợp 1:** Chọn $x$ để cộng vào tổng.

  Để dễ hình dung, thì ta lấy giá trị $x$ lớn nhất có thể có mà đề bài cho luôn, là số $x = 10000$. Khi đó, số điểm mà ta sẽ được cộng thêm là $10000 * n$, với $n$ là số lượng các phần tử có cùng giá trị $10000$ ở trong mảng. Đồng thời, ta phải xóa đi tất cả các phần tử có giá trị lân cận, đó là $9999$.

  Như vậy, tổng điểm lớn nhất mà chúng ta có thể thu được đối với giá trị $10000$, chính là $10000 * n$ cộng với tổng điểm lớn nhất mà chúng ta có thể thu được với giá trị $9998$ (không có $9999$ vì nó đã bị xóa rồi).

  <math>
  F(10000) = F(9998) + (10000 * n)
  </math>

  Giá trị của $F(9998)$ là bao nhiêu thì chúng ta vẫn chưa biết. Cứ để đó đã.

- **Trường hợp 2:** Không chọn $x = 10000$ để cộng vào tổng. Khi đó chắc chắn chúng ta sẽ phải chọn một giá trị lân cận của $x$, đó là $9999$.

  <math>
  F(10000) = F(9999)
  </math>

Việc chọn trường hợp 1 hay 2 tùy thuộc vào kết quả tốt nhất mà nó mang lại, ở đây là **trường hợp nào cho kết quả lớn hơn**:

<math>
F(10000) = max\Big(F(9999), F(9998) + (10000 * n)\Big)
</math>

Thế là ta đã tìm được công thức truy hồi để tính $F(10000)$ từ $F(9999)$ và $F(9998)$. Hay viết tổng quát ra thành:

<math>
F(x) = max\Big(F(x-1), F(x-2) + x * count(x)\Big)
</math>

## Implementation

Có công thức truy hồi rồi thì ta dễ dàng implement được thuật toán để giải bài này. Sau đây là bài giải bằng JavaScript của mình, và bằng Golang của anh linxGnu.

Mấu chốt của việc giải một bài toán Quy hoạch động chính là việc tìm ra công thức truy hồi, hoặc dạng đệ quy của bài toán (thường là thể hiện tính chất của bài toán từ phương án tổng quát đến cụ thể - top-down), sau đó, ta chỉ việc implement lại công thức đó theo thứ tự ngược lại (từ trường hợp cụ thể đến tổng quát - bottom-up) [^3].

**JavaScript**

```javascript
const deleteAndEarn = (nums) => {
  if (!nums.length) {
    return 0;
  }

  let maxPossible = 10000;

  let points = Array.from(Array(maxPossible + 1)).map(x => 0);
  for (let i = 0; i < nums.length; i++) {
    points[nums[i]] += nums[i];
  }
  
  let F = Array.from(Array(maxPossible + 1)).map((x, i) => points[i]);
  for (let i = 2; i <= maxPossible; i++) {
    F[i] = Math.max(F[i-1], F[i-2] + points[i]);
  }

  return F[maxPossible];
};
```

Bài giải bằng Golang có sử dụng một vài trick khá hay.

**Golang**

```go
func deleteAndEarn(nums []int) int {
    if nums == nil || len(nums) == 0 {
        return 0
    }
    
    // use counter sort instead
    counter := make([]int, 10001)
    for _, v := range nums {
        counter[v]++
    }
    
    // using counter for Dynamic Programing formula
    // f[i] = max(f[i-2] + counter[i] * i, f[i-1])
    if counter[2] <<= 1; counter[2] < counter[1] {
        counter[2] = counter[1]
    }
    for i := 3; i <= 10000; i++ {
        if counter[i] = counter[i-2] + counter[i] * i; counter[i] < counter[i-1] {
            counter[i] = counter[i-1]
        }
    }
    
    return counter[10000]
}
```

## Comments

Thay cho phần kết luận, mình xin trích dẫn một vài ý từ cuốn **The Algorithm Design Manual** về Quy hoạch động mà mình cho là rất có ích:

- Dynamic programming is best learned by carefully studying examples until things start to click.
- Ultil you understand dynamic programming, it seems like magic.
- You must figure out the trick before you can use it.

Và đây là trick:

- Dynamic programming is a technique for efficiently implementing a **recursive algorithm** by **storing partial results**.
- The trick is: seeing whether the naive recursive algorithm computes the same subproblems over and over again.
- If so, storing the answer for each subproblem in a table to lookup instead of recomputing.
- Start with a recursive algorithm or definition. Only once we have a correct recursive algorithm, do we worry about speeding it up by using a result matrix.

---

**Notes**

- [^1]: Nghe đồn ông này từng đi ra đề thi quốc gia lận đó. Nguồn: tin đồn
- [^2]: Nhóm này có rule rất đáng ghét, ví dụ như trong 1 tuần mà ông nào không làm bài thì coi như mất toi 10 USD, trích từ [rule của nhóm](https://github.com/ruby-vietnam/hardcore-rule/tree/master/algorithms). Không nghiêm túc thì không tiến bộ được.
- [^3]: Nên đọc thêm Chương 8 của quyển [The Algorithm Design Manual](http://www.algorist.com/), phần giới thiệu về Dynamic Programming có nói rất rõ về tính chất này và rất dễ hiểu.
