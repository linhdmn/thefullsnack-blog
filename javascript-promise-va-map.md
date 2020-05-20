---
title: Promise, Async/Await và Map/Reduce
published: true
date: 2018-09-27 15:31:19
tags: javascript
description: Vấn đề nhỏ với Promise khi dùng với Map/Reduce và async/await
image:
---
Có một cái sai mà người ta thường hay mắc phải khi làm việc với `async/await`, đó là khi kết hợp nó với các hàm `Array.map/Array.reduce`, họ hiểu sai tác dụng của `async/await`, dẫn tới việc kết quả trả về không như ý.

Giả sử ta có hàm `parseUrl(<url>)` nhận vào một chuỗi (là địa chỉ của một RSS feed), và trả về danh sách các item có trong RSS feed đó, mà ở đây chúng ta biểu diễn bằng một mảng `[article]`, kết quả trả về thông qua `Promise`, nội dung hàm này như nào thì không ảnh hưởng nhiều tới bài viết, nên chả cần ghi ra làm gì:

```javascript
// parseUrl :: string -> Promise [article]
```

Khi sử dụng `await`, ta có thể lấy kết quả của `Promise` đó như thế này, có thể minh họa bằng sơ đồ:

```javascript
await parseUrl("https://thefullsnack.com/rss.xml");
```

![](img/javascript-promise-parseurl.png)

Tiếp theo, là hàm `getArticles([urls])` nhận vào một mảng nhiều RSS feed URL, và trả về nhiều mảng chứa danh sách các item tương ứng với từng feed, theo như hiểu biết về cách làm việc với `Promise` thông qua hàm `await` như trên, mà `await` chỉ sử dụng được bên trong các hàm `async`, vậy thì thêm `async` vào, ta có thể dễ dàng implement như sau:

```javascript
// getArticles :: [string] -> [[article]]
async function getArticles(sources) {
    return await sources.map(async (url) => {
        return await parseUrl(url);
    });
}
```

Chúng ta expect hàm `getArticles` hoạt động theo sơ đồ bên dưới:

![](img/javascript-promise-getarticles.png)

Tuy nhiên, khi chạy, thì hàm trên không trả về kết quả như mong đợi:

```javascript
getArticles([
    "https://thefullsnack.com/rss.xml",
    "https://news.ycombinator.com/rss"
])

// Output:

[ Promise { [ [Object] ] },
  Promise { [ [Object] ] } ]
```

Chúng ta **tưởng** rằng, sử dụng lệnh `await` sẽ giúp trả về kết quả được `resolved` của một `Promise`, mà cụ thể ở đây bên trong hàm `source.map()`, chúng ta có thể nhận được một mảng chứa kết quả của các promise `parseUrl`. Nhưng trong trường hợp này, kết quả trả về lại là các `Promises`, vậy chúng ta đã làm sai ở chỗ nào?

---

Hãy xem một hàm `async` hoạt động ra sao:

```javascript
async function increase(a) {
    return a + 1;
}

increase(1);

// Output:

Promise { 2 }
```

Hàm `async` luôn trả về một `Promise`, viết type singature theo kiểu mấy ngôn ngữ functional là:

```javascript
// async increase :: number -> Promise number
```
![](img/javascript-promise-async.png)

Còn từ khóa `await` thì có tác dụng dừng việc thực thi code lại và chờ lấy trực tiếp giá trị trả về trong một `Promise`:

```javascript
// await Promise number -> number
let two = await increase(1);
// two = 2
```

![](img/javascript-promise-await.png)

Nếu không dùng `await` thì ta phải dùng `.then()`, là cách truyền thống để nhận giá trị trả về của một `Promise`, giá trị trả về chỉ có thể sử dụng được trong scope (phạm vi) của `.then()`, và không biết sử dụng nó trực tiếp ở scope hiện tại như thế nào luôn.

```javascript
let two = increase(1).then(n => {
   // n = 2
   ...
})
// two = Promise
```

---

Quay trở lại ví dụ đầu bài, hãy cùng xem lại hàm `getArticles` trả về kết quả như thế nào. Chúng ta sẽ đi từ trong ra ngoài.

```javascript
async function getArticles(sources) {
    return await sources.map(async (url) => {
        return await parseUrl(url);
    });
}
```

Đầu tiên là hàm xử lý dữ liệu trong khối lệnh `sources.map()`:

```javascript
// async f :: string -> Promise [article]
async (url) => {
    return await parseUrl(url);
}
```

Ngay tại đây chúng ta thấy, hàm callback của `sources.map()` trả về một `Promise` chứ không phải là một mảng các `article` như dự tính ban đầu.

<div class="mute">
■ :chicken: Mặc dù chúng ta sử dụng await để lấy kết quả trả về từ parseUrl() (vốn là một Promise), tuy nhiên vì nằm trong một hàm async, kết quả này rốt cuộc cũng bị wrap lại vào bên trong một Promise. :joy:
</div>

Điều này dẫn đến việc, kết quả của câu lệnh `map` là một mảng các `Promises`, thay vì là mảng của các mảng `[article]` như ta nghĩ.

![](img/javascript-promise-map.png)

Và kết quả là hàm `getArticles()` trả về một mảng các `Promises`, và mỗi một `Promise` trong mảng này lại chứa các `[article]` của chúng ta:

![](img/javascript-promise-real-getarticles.png)

Vậy nên, cách để giải quyết vấn đề trên là, sử dụng `Promise.all()` để lấy toàn bộ kết quả trả về từ các `Promise` có trong `sources.map()`, sau đó mới đưa ra cho hàm `getArticles()`:

```javascript
async function getArticles(sources) {
    let promises = sources.map(async (url) => {
        return await parseUrl(url);
    });
    return await Promise.all(promises);
}

let url = await getArticles([
    "https://thefullsnack.com/rss.xml",
    "https://news.ycombinator.com/rss"
])

// Output:

[ [ { title: 'Giấy với bút',
      link:  'https://thefullsnack.com/posts/paper-and-pen.html' },
    { title: 'Vài ghi chép về V8 và Garbage Collection',
      link:  'https://thefullsnack.com/posts/javascript-v8-notes.html' },
    ...
  ],
  [ { title: 'Elon Musk Accused by SEC of Misleading Investors in August Tweet',
      link:  'https://news.ycombinator.com/item?id=18088099' },
    { title: 'People can die from giving up the fight',
      link:  'https://news.ycombinator.com/item?id=18083509' },
    ...
  ] ]
```

---

Bài học rút ra ở đây là gì? Đó là, luôn luôn đọc kĩ tài liệu trước khi cắm đầu sử dụng, và quan trọng nhất là không được đoán mò :smirk:, `async/await`, cũng giống như mọi khái niệm khác trong JavaScript, luôn cực kì rắc rối và khó hiểu cho tới chừng nào chúng ta... hiểu nó.

Mình biết điều này vì chính mình cũng đã lười đọc tài liệu, dẫn đến làm sai, nên mới có bài viết này :joy:
