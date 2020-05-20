---
title: Algorithm in Frontend - Kỳ 3: Hashmap
published: true
date: 2017-12-16 18:05:01
tags: code, algorithm, frontend
description: Frontend và thuật toán, hai khái niệm vốn được nhiều người cho là không liên quan đến nhau, thực tế có đúng như vậy không?
image: https://thefullsnack.com/posts/img/frontend-algorithm-001-code-monkey-2.png
---
Hôm nay nói về một ứng dụng của Hashmap trong việc optimize một số thuật toán thường gặp trên Frontend.

Giả sử có một mảng `languages` lưu danh sách các ngôn ngữ lập trình:

```javascript
const languages = ["c++", "java", "javascript", "ruby", "rust", "golang"];
```

Và một hàm `getLanguages` nhận vào một mảng `input`, trả ra mảng gồm các phần tử vừa có trong `input` vừa có trong `languages` (nói theo kiểu tập hợp thì là phép giao hai tập $\texttt{input} \cap \texttt{languages}$):

```javascript
const normalize = text => text.trim().toLowerCase();

const getLanguages = (input) => {
  return languages.filter(searchLang => {
    return !(input.findIndex(inputLang =>
      normalize(inputLang) === normalize(searchLang)) === -1);
    });
};
```

Về mục đích của hàm trên, thì hãy tưởng tượng bạn đang làm một trang web về tuyển dụng, cho phép user (là ứng viên) nhập vào các ngôn ngữ mình sử dụng, có người sẽ nhập `Ruby`, có người nhập `ruBy`, `ruby`,... hàm trên sẽ filter các input sai lệch đó và trả về input đúng với database nhất.

Ví dụ:

```javascript
getLanguages(["c++  ", "JavaScript", "GoLang"]);
// Output:
// [ "c++", "javascript", "golang" ]
```

Hàm `getLanguages` sẽ có độ phức tạp trong trường hợp xấu nhất là $O(n*k)$, hoặc nếu  tập `input` có kích thước bằng tập `languages` luôn thì độ phức tạp sẽ là $O(n^2)$.

Sở dĩ tốn kém như vậy là vì chúng ta dùng array, và với array, để tìm ra tập giao nhau, hay nói cách khác, để tìm một phần tử `languages[i]` bất kì có cùng giá trị tương ứng với một phần tử `input[j]`, ta không có cách nào khác là phải duyệt qua toàn bộ cả 2 mảng.

Vậy đây có thể là mấu chốt để chúng ta optimize thuật toán trên. Liệu có cách nào để từ giá trị của một phần tử `input[j]`, ta có thể truy xuất ngay đến phần tử tương ứng nếu có của `languages` không? (truy xuất với độ phức tạp $O(1)$).

Một kiểu dữ liệu phù hợp với tính chất truy xuất trên đó là Hashmap, vậy nếu dùng hashmap để thực hiện bước lookup, ta có thể loại bỏ được một vòng lặp lồng bên trong.

Nhưng dữ liệu cho sẵn (ở đây là mảng `languages`) là một mảng, ta cần phải chuyển nó thành hashmap trước:

```javascript
const languagesLookup = Object.create(null);
languages.forEach(lang => { languagesLookup[normalize(lang)] = 1; });
```

Như vậy ta có thể dễ dàng implement lại thuật toán của `getLanguages` thành:

```javascript
const getLanguages = (input) => {
  return input.reduce((array, lang) => {
    let key = normalize(lang);
    if (languagesLookup[key]) {
      array.push(key);
    }
    return array;
  }, []);
};
```

Bằng cách lợi dụng tính chất truy xuất nhanh và ít tốn kém của Hashmap, ta đã có thể loại bỏ việc phải chạy 2 vòng lặp lồng nhau, và optimize thuật toán ban đầu từ $O(n*k)$ về $O(n+k)$, hay trong trường hợp $k = n$ thì ta có thể nói là độ phức tạp giảm từ $O(n^2)$ về $O(n)$.
