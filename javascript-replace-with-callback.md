---
title: Hàm replace() xài với callback
published: true
date: 2019-01-04 00:45:19
tags: javascript
description: Dùng hàm replace() với callback để giải quyết vấn đề replace chuỗi theo cách linh hoạt hơn, trong sáng hơn, và dễ maintain hơn.
image:
---

Hàm `replace()` của một chuỗi trong JavaScript có một biến thể dùng kèm với một callback. Cái này thì không mới, [đã ghi trong document từ rất lâu rồi](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace), nhưng kì thực thì mình chưa bao giờ cần phải replace thứ gì phức tạp tới mức phải dùng tới callback, nên cũng chả quan tâm gì mấy. Chỉ đơn giản là theo cú pháp:

```
input.replace([từ khóa], [nội dung thay thế])
// ví dụ
"hello".replace("ll", "r") // -> "hero"
// hoặc
"hello".replace(/l/g, "r") // -> "herro"
```

Cú pháp dùng callback thì có dạng:

```
input.replace([từ khóa], (matched, index, original) => {
    ...
})
```

Hàm callback sẽ được gọi mỗi lần chúng ta tìm ra một nội dung được khớp với từ khóa, với 3 tham số là `matched` (nội dung trùng khớp), `index` (vị trí của `matched` trong chuỗi input) và `original` (chính là chuỗi input). Giá trị trả về của callback sẽ được replace vào vị trí xuất hiện của `matched`.

Sử dụng callback như thế này sẽ giúp cho chúng ta thực hiện được các thao tác phức tạp hơn nhiều lần so với cách replace thông thường.

**Ví dụ:** Chúng ta cần replace kí tự `"a"` xuất hiện ở _**vị trí thứ 2**_ trong chuỗi `"abcabc"` thành kí tự `"$"`.

Trong trường hợp này thì bạn sẽ viết regex như thế nào nếu dùng với cú pháp mặc định ở đầu bài? Rồi nếu yêu cầu thay đổi từ vị trí thứ 2 sang một ví trí thứ n nào đó thì sao? Đoạn regex bạn viết ra liệu có phức tạp không? Sau vài tháng liệu bạn đọc lại đoạn regex đó thì có còn hiểu gì nữa không? Hay lỡ mà bạn nghỉ việc, rồi thằng developer xấu số nào đó nhận maintain cái đống code của bạn, nó có gọi điện cho bạn để chửi, hay thậm chí là dọa giết luôn không? :think-hard:

Còn nếu dùng callback của hàm `replace()`, bạn chỉ cần viết thế này:

```
"abcabc".replace(/a/g, (matched, index, original) => {
    if (index !== 0) {
        return "$";
    } else {
        return matched;
    }
});
```

Hoặc viết theo cách tổng quát hơn cho trường hợp vị trí thứ n bất kì, thì ta phải đếm, như này:

```
const replaceNth = (input, search, replacement, nth) => {
    let occurrence = 0;
    return input.replace(search, matched => {
        occurrence++;
        if (occurrence === nth) return replacement;
        return matched;
    });
};
```

Dùng như này:

```
replaceNth("abcabcabcabcabcabcabc", /a/g, "$", 5)

Output: "abcabcabcabc$bcabcabc"
         -  -  -  -  ^
         1  2  3  4  5
```

Mặc dù dài dòng, nhưng cách viết dùng callback trên rõ ràng là dễ hiểu hơn và có khả năng maintainable cao hơn, cho chính người viết ra nó hoặc người khác phải làm việc với nó.
