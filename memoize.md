---
title: Kĩ thuật Memoize cải thiện performance
published: true
date: 2017-03-21 10:42:09
tags: javascript, memoize, performance, algorithm
description: Memoize là một kĩ thuật cache lại giá trị trả về của các hàm dựa trên tham số truyền vào nó.
image:
---
Memoize là một kĩ thuật cache lại giá trị trả về của các hàm dựa trên tham số truyền vào nó.

Kĩ thuật này có thể áp dụng trên mọi ngôn ngữ lập trình, trong bài viết này mình chỉ lấy JavaScript ra làm ví dụ.

## Đặt vấn đề: Bài toán tìm số Fibonacci

Đối với bài toán tìm số Fibonacci, chúng ta đều biết thuật toán đơn giản nhất để implement nó là sử dụng đệ quy:

<math>
F_{n}=F_{n-1}+F_{n-2}
</math>

Hay diễn giải bằng code như sau:

```
const fibo = (n) => {
  if (n === 0) return 0;
  if (n === 1) return 1;
  return fibo(n - 1) + fibo(n - 2);
}
```

Nhược điểm của giải pháp trên rất rõ ràng, đó là thời gian tính toán. Đối với mỗi lần tính toán, hàm `fibo` phải thực hiện tính toán lại từ đầu (từ `n = 0`) đến `n`. Trong quá trình đó một giá trị `fibo(n)` sẽ bị tính đi tính lại liên tục, một sự lãng phí không hề nhỏ.

Thời gian chạy của thuật toán trên cho kết quả `fibo(50)`:

```
// fibo(50);
➜  time node fibo.js

193.97s user 
0.56s   system 
99%     cpu 
3:15.24 total
```

Và như ta đã nhận thấy, thì phần lớn thời gian tiêu tốn vào việc tính đi tính lại các giá trị trùng lặp, vậy để cải thiện, ta cần tìm một cách nào đó để lưu lại giá trị của từng phép tính `fibo(n)`, và tránh việc tính lại từ đầu mỗi khi chạy. Cách này gọi là **caching**.

Vì đối với mỗi số `n` bất kì, ta luôn luôn chỉ có duy nhất một giá trị sau khi tính toán với hàm `fibo(n)`. Vậy nên ta có thể tạo ra một mảng tên là `cache` để lưu lại các giá trị `fibo(n)` đã tính toán, ở lần chạy tiếp theo, chỉ việc lấy nó ra và sử dụng, không cần phải tính lại.

```
const cache = [];

const fibo = (n) => {
  if (n === 0) 
    cache[n] = 0;
  else if (n === 1) 
    cache[n] = 1;
  else 
    cache[n] = cache[n] || (fibo(n - 1) + fibo(n - 2));
  return cache[n];
}
```

Kết quả thời gian chạy `fibo(50)` được cải thiện đáng kể:

```
// fibo(50);
➜  time node fibo.js

0.05s user 
0.02s system 
98%   cpu 
0.072 total
```

## Memoize

Ví dụ trên cho chúng ta thấy được tầm quan trọng của việc cache trong việc cải thiện tốc độ xử lý của một ứng dụng.

Tuy nhiên, cứ phải tạo một biến toàn cục `cache` một cách thủ công như vậy không phải là một giải pháp hay, chưa kể đến việc cần cache nhiều thứ trong một ứng dụng, chúng ta cần một phương pháp tổng quát hơn, và hiệu quả hơn. Đó là memoize.

Memoize khác với các kĩ thuật cache thông thường ở chỗ: **Nó được dùng để cache giá trị trả về của một hàm, và cache được tạo ra dựa trên tham số của hàm đó.**

Sau đây là một cách implement đơn giản cho hàm `memoize`:

```
const memoize = (func) => {
  let cache = {};
  let that = this;
  return (...args) => {
    let key = JSON.stringify(args);
    if(cache[key]) {
      return cache[key];
    }
    else {
      let val = func.apply(that, args);
      cache[key] = val;
      return val;
    }
  }
};
```

Hàm `memozie` nhận vào tham số là một hàm, tạo cache dựa trên các tham số truyền vào của hàm đó. Khi được gọi thì nó sẽ trả về giá trị được cache nếu có, nếu chưa có giá trị cache thì chạy chính hàm được truyền vào và lưu lại giá trị trả về.

Cực kì đơn giản đúng không? Giờ chúng ta thử implement lại hàm `fibo` ở đầu bài dùng `memoize`:

```
const memfibo = memoize((n) => {
  if (n === 0) return 0;
  if (n === 1) return 1;
  return memfibo(n - 1) + memfibo(n - 2);
});
```

Phần logic vẫn được giữ nguyên so với cách implement đầu tiên, nhưng vẫn sử dụng được cache, đó là ưu điểm của việc dùng `memoize`.

Chạy thử hàm `memfibo` vừa tạo với tham số `n = 100` luôn coi sao:

```
// memfibo(100);
➜  time node fibo.js

0.06s user 
0.02s system 
97%   cpu 
0.077 total
```

Quá xịn :D

## Các implement khác của memoize

Trong thực tế, có thể bạn sẽ không đủ tự tin để sử dụng hàm `memoize` do chính mình implement, hoặc team/sếp không tin tưỏng bạn, ok, không sao, mặc dù bị tổn thưong nhưng không nên vì thế mà nản.

Chúng ta có thể sử dụng một vài implement khác như:


- Lodash Memoize ([https://npmjs.com/package/lodash.memoize](https://npmjs.com/package/lodash.memoize))
- Fast-memoize ([https://npmjs.com/package/fast-memoize](https://npmjs.com/package/fast-memoize))

Ngoài ra, các bạn có thể tham khảo thêm bài viết [How I wrote the fastest JavaScript memoization library](https://community.risingstack.com/the-worlds-fastest-javascript-memoization-library/) để có cái nhìn sâu hơn về các kĩ thuật implement/tối ưu hóa cho memoize.

## Bonus: Sử dụng memoize trong Angular filter

Một trưòng hợp mà bạn có thể sử dụng memoize khá là hiệu quả đó là khi implement một `filter` trong Angular. Ví dụ:

```
angular.module('app').filter('evenNumbers', function() {
  return function(items) {
    return items.reduce(function(result, item) {
      return (item % 2 === 0);
    }, []);
  };
});
```

Filter trên nhận vào một mảng và trả về một mảng mới, nhưng khi sử dụng filter trên bạn sẽ gặp lỗi:

```
Error: [$rootScope:infdig] 10 $digest() iterations reached. Aborting!
```

Nguyên nhân của lỗi trên là vì: hàm `reduce` trả về một mảng mới mỗi lần bạn chạy filter `evenNumbers`, khiến cho Angular "hiểu lầm" kết quả trả về đã đưọc thay đổi và nó phải chạy một digest cycle mới để kiểm tra, và việc chạy này xảy ra liên tục.

Khi gặp những trưòng hợp như vậy, bản chỉ cần đặt hàm filter vào bên trong `memoize` để cache lại giá trị trả về là xong:

```
angular.module('app').filter('evenNumbers', function() {
  return memoize(function(items) {
    return items.reduce(function(result, item) {
      return (item % 2 === 0);
    }, []);
  });
});
```

## Lời cuối: đồ xịn nhưng dùng nhiều thì hết xịn

Có một trưòng hợp mà dù có sử dụng memoize thì cũng vô dụng, đó là khi dữ liệu trả về của một hàm nó thay đổi không phụ thuộc vào tham số truyền vào của hàm đó. Ví dụ dữ liệu tracking dạng time series, hay giá chứng khoán, hay danh sách status trên tưòng nhà đứa bạn thích trên Facebook chẳn hạn.

Và thêm nữa, vì memoize cũng là caching, tức là dữ liệu tính toán xong được lưu lại và nằm đó luôn, không mất đi đâu cả, cho nên bạn phải nghĩ tới vấn đề khi app chạy trong một khoản thời gian dài, thì dung lưọng bộ nhớ cũng vì thế mà tăng lên. Cho nên cần phải giải phóng bộ nhớ cache của hàm memoize sau một khoản thời gian nhất định, nếu không sẽ dẫn tới tràn bộ nhớ hoặc memleak.

Vì sử dụng memoize tức là bạn đang đánh đổi memory để lấy tốc độ, chứ không có cái gì là free cả, nên nếu biết chừng mực và dùng đúng lúc đúng chỗ thì sẽ đem lại hiệu quả cao, còn ngược lại, lạm dụng quá mức thì hậu quả sẽ khó lường. Cho nên gì gì thì cũng phải hiểu để xài nhau cho tốt hơn, nhé :D 
