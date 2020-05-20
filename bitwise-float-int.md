---
title: Chuyển số Float thành Int dùng các phép bitwise
published: true
date: 2018-05-04 14:30:37
tags: algorithm, math, bitwise
description: 
image:
---
Gần đây có một trick mà mình rất hay dùng, đó là sử dụng hai phép bitwise **NOT** `~~` để chuyển nhanh một số kiểu **float** thành **int**, thay cho việc dùng hàm `Math.floor`:

```javascript
        ~~(5.423451) == 5 // true
Math.floor(5.423451) == 5 // true
```

Ngoài hiệu quả về mặt performance ra, trick này có thể được sử dụng trong các buổi phỏng vấn để... làm màu :joy:. Tuy nhiên, việc sử dụng phải đi kèm với việc hiểu và giải thích được cơ chế hoạt động của phép tính này.

Vậy vì lý do gì mà phép tính này có thể chuyển một số kiểu **float** thành **int**?

Về mặt bản chất, tất cả các kiểu số trong JavaScript đều chiếm 64-bit trong bộ nhớ theo chuẩn IEEE 754 [^1].

![](img/javascript-float.png)

Để tiết kiệm bộ nhớ và hiệu năng, với đa số các JavaScript engine (như là V8, SpiderMonkey,...), nếu một biến $x$ kiểu số có giá trị nguyên vừa đủ nhỏ (ví dụ $-2^{31} \le x \le 2^{31}$), thì biến đó sẽ được thể hiện dưới dạng một số kiểu 32-bit integer[^2]. Chừng nào con số trên đủ lớn, hoặc hoặc không phải là số nguyên nữa thì nó sẽ được tự động chuyển về dạng 64-bit.

Theo đặc tả ECMAScript [^3], khi thực hiện các phép toán bitwise, các toán hạng sẽ tự động bị chuyển về kiểu 32-bit integer [^4] [^5].

Tức là khi ta thực hiện **bất kỳ** một phép toán bitwise nào trên một số, thì kết quả trả về luôn là kiểu 32-bit integer.

Và thường thì chúng ta sẽ sử dụng các phép bitwise vô nghĩa, để tránh làm thay đổi giá trị của số ban đầu, ví dụ:

```javascript
~~(5.423451) == 5 // Double not
5.423451 | 0 == 5 // Or với 0
5.423451 << 0 == 5 // Shift 0
5.423451 >> 0 == 5 // Shift 0
```

---

Chúng ta có thể sử dụng trick này cho rất nhiều tình huống. Ví dụ như thuật toán kiểm tra một số có phải là lũy thừa của 4 hay không ([Leetcode #342](https://leetcode.com/problems/power-of-four/description/)).

Nếu một số nguyên $n \in Z$ là lũy thừa bậc 4 của một số $x$ nào đó, thì $x$ cũng phải là một số nguyên ($x \in Z$). Hay nói cách khác, giá trị logarithm cơ số 4 của $n$ cũng phải là một số nguyên ($log_4n \in Z$).

Để tính logarithm cơ số bất kì, ta có thể sử dụng [công thức chuyển đổi cơ số](https://en.wikipedia.org/wiki/Logarithm#Change_of_base) để đưa về cùng một dạng logarithm mà máy tính hỗ trợ sẵn, ví dụ $log_{10}$:

<math>
log_ab = \displaystyle{\frac{log_{10}b}{log_{10}a}}
</math>

Thuật toán của chúng ta sẽ được implement như sau:

```javascript
var isPowerOfFour = function(num) {
    let lg4 = Math.log(num) / Math.log(4);
    return lg4 === (~~lg4);
};
```

**Đọc thêm**

[^1]: [The Internal Representation of Numbers](http://speakingjs.com/es5/ch11.html#number_representation), Speaking JavaScript, Chapter 11

[^2]: [Integers in JavaScript](http://speakingjs.com/es5/ch11.html#integers), Speaking JavaScript, Chaper 11

[^3]: [Bitwise NOT Operator](https://www.ecma-international.org/ecma-262/8.0/index.html#sec-bitwise-not-operator), ECMAScript 2017 Language Specification

[^4]: [JavaScript: Bitwise Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Signed_32-bit_integers), MDN Web Docs

[^5]: [32-bit Integers via Bitwise Operators](http://speakingjs.com/es5/ch11.html#integers_via_bitwise_operators), Speaking JavaScript, Chapter 11
