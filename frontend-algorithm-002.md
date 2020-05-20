---
title: Algorithm in Frontend - Kỳ 2: Tree và Menu
published: true
date: 2017-10-15 17:38:18
tags: code, frontend, algorithm
description: Nhắc đến cây thì hẳn chúng ta nghĩ ngay đến DOM và các thao tác trên đó, ví dụ như tìm kiếm một element trên DOM, thêm/xóa element,... tuy nhiên các thao tác này chúng ta thường sử dụng DOM API có sẵn của trình duyệt và bài này cũng không có ý định nói đến phần đó :laughing:
image: https://thefullsnack.com/posts/img/c-programmer.png
---
Tiếp tục với sê ri [Algorithm in Frontend](https://thefullsnack.com/posts/frontend-algorithm-001.html), kì này nói về một loại cấu trúc dữ liệu được dùng thường xuyên trên frontend, đó là **Kiểu dữ liệu dạng cây (tree)**.

Nhắc đến cây thì hẳn chúng ta nghĩ ngay đến DOM và các thao tác trên đó, ví dụ như tìm kiếm một element trên DOM, thêm/xóa element,... tuy nhiên các thao tác này chúng ta thường sử dụng DOM API có sẵn của trình duyệt và bài này cũng không có ý định nói đến phần đó :laughing:

## Bài toán cần giải quyết

Thực ra đây là một câu hỏi phỏng vấn cho vị trí Frontend Engineer tại công ty **G** cũng khá là có tiếng tăm (vì họ bắt mình ký NDA không được tiết lộ nội dung buôi phỏng vấn, nhưng mà để viết bài này thì phải lộ đề rồi, nên thôi mình không tiết lộ tên công ty đó vậy :grin: hình như đây là lần thứ 2 mình nhắc đến NDA rồi, mấy cái công ty cứ thích dùng cái này để bịt miệng người ta quài).

Đề bài như này. Cho một cục dữ liệu JSON có dạng:

**menu.json**
```json
[
  {
    "name": "Electronics",
    "items": ["Laptop", "Headphones"]
  },
  {
    "name": "Headphones",
    "items": ["Shure", "Bose", "JLB"]
  },
  {
    "name": "Cars",
    "items": ["SUV", "Sedan", "Sports"]
  },
  {
    "name": "SUV",
    "items": ["Honda", "Ferrari"]
  },
  {
    "name": "Laptop",
    "items": ["Thinkpad"]
  }
]
```

Hãy xây dựng một component in ra một menu đa cấp (multi-level menu) dựa trên dữ liệu đã cho. Đối với dữ liệu mẫu ở trên thì cấu trúc của menu sẽ là:

```text
  |- Electronics
  |    |- Headphones
  |    |   |- Shure
  |    |   |- Bose
  |    |   |- JLB
  |    |- Laptop
  |    |   |- Thinkpad
  |- Cars
  |    |- SUV
  |    |   |- Honda
  |    |   |- Ferrari
  |    |- Sedan
  |    |- Sports
```

Câu hỏi này gây ấn tượng mạnh với mình vì lúc đó là lần đầu tiên mình gặp một câu hỏi phỏng vấn núp bóng dưới dạng một ứng dụng thực tiễn rõ ràng như thế này.

## Phân tích đề bài

Mặc dù đề bài yêu cầu _xây dựng một component in ra một menu đa cấp_, nhưng bản chất bài toán vẫn là **xây dựng thuật toán chuyển cục JSON kia về dạng cây**.

_Nói thì dễ, đến lúc vào phỏng vấn thì mặt mình tái mét, dành hết 40 phút để implement cái cơ chế tạo và mount một custom component theo kiểu Angular Directive, trong khi không đả động gì đến cái đề bài, kết quả là fail dập mặt :sweat:_

Nhìn vào dữ liệu input (tạm gọi là $I$) ta có thể thấy đây là một mảng "phẳng" (flat array), và mỗi phần tử của mảng này có thể có hoặc không có mối liên hệ nào với nhau. Nếu một phần tử $I_j$ là "con" của $I_i$ thì tên $I_j\texttt{.name}$ sẽ có mặt trong mảng $I_i\texttt{.items}$.

Nhiệm vụ của chúng ta sẽ biến mảng phẳng này thành một cây dữ liệu, dựa trên mối quan hệ vừa đề cập.

## Cấu trúc dữ liệu

OK, vậy thì đầu tiên ta phải nghĩ về cấu trúc dữ liệu của một Node trên cây, sẽ có dạng như sau:

**pseudo code**
```javascript
struct Node {
  name: String,
  items: Array<Node>
}
```

Với cấu trúc trên, một Node cha sẽ có các Node con nằm trong trường `items`, và với một Node lá, không có Node con nào, thì trường `items` chỉ là một mảng rỗng.

Như vậy, nếu $\texttt{Thinkpad}$ là một Node lá, và cũng là con của $\texttt{Laptop}$ thì cây của chúng ta sẽ có dạng:

```javascript
Node {
  name: "Laptop",
  items: [
    Node {
      name: "Thinkpad",
      items: []
    }
  ]
}
```

## Xây dựng thuật toán

Ý tưởng của thuật toán sẽ gồm 2 công đoạn, giống như chơi xếp hình vậy:

- **Màn dạo đầu:** để thống nhất về kiểu dữ liệu, ta đưa mảng input $I$ thành một mảng có mỗi phần tử là một cây riêng biệt, có gốc (root) chính là phần tử $I_i$ đang xét, và các node lá là các phần tử của $I_i\texttt{.items}$.
- **Xếp hình:** gọi là lắp ráp thì đúng hơn, ở bước này ta sẽ ráp nối các cây lại với nhau theo kiểu, cây nào con cây nào thì cắm vào cây đó. Kết thúc việc lắp ráp thì chúng ta sẽ có một cây hoàn chỉnh như đề bài yêu cầu.

Thuật toán đầy đủ được mô tả như sau:

- **Bước 1:** Duyệt qua tất cả các phần tử của mảng $I$
- **Bước 2:** Với mỗi phần tử $I_i$ (còn gọi là một cây $I_i$), duyệt qua tất cả các phần tử của mảng $I_i\texttt{.items}$ (các cây con của $I_i$).
- **Bước 3:** Gọi $\texttt{node}$ là phần tử $I_i\texttt{.items}_j$ đang xét. Tìm trong mảng $I$ một phần tử $I_t$ sao cho $I_t\texttt{.name} == \texttt{node}$.
- **Bước 3a:** Nếu tìm thấy một phần tử $I_t$ thỏa điều kiện trên thì thay thế $\texttt{node}$ thành $I_t$. Đánh dấu $I_t$ thành một cây cần xóa.
- **Bước 3b:** Nếu không thì biến $\texttt{node}$ thành một lá của cây $I_i$
- **Bước 4:** Xóa tất cả các cây $I_i$ đã được đánh dấu trong mảng $I$. Mảng thu được chính là mảng kết quả.

Khác với các ngôn ngữ lập trình khác, vì JavaScript là dynamic typing, ta có thể dễ dàng thay đổi giữa nhiều kiểu dữ liệu cho một biến, chính vì thế, rất có khả năng bạn sẽ bị confuse khi đọc đến bước **3** và **3a** :see_no_evil:

Ở bước **3**, một giá trị $\texttt{node} = I_i\texttt{.items}_j$ sẽ mang kiểu `string`, đến bước **3a** ta lại gán $\texttt{node}$ thành một giá trị $I_t$ (là một cây).

Nếu ngôn ngữ mà các bạn sử dụng không phải JavaScript mà là một ngôn ngữ strong typing nào đó ví dụ như Go hay Rust hay C/C++, thì thuật toán ở trên cần phải được sửa lại.

Còn bây giờ thì implement thôi:

```
const buildTree = (nodes) => {
  for (let i = 0; i < nodes.length; i++) {
    for (let j = 0; j < nodes[i].items.length; j++) {
      let node = nodes[i].items[j];
      let found = nodes.find(n => n.name == node);
      if (found) {
        nodes[i].items[j] = Object.assign({}, found);
        found.removed = true;
      } else {
        nodes[i].items[j] = {
          name: node,
          items: []
        };
      }
    }
  }

  return nodes.reduce((arr, item) => {
    if (!item.removed) {
      arr.push(item);
    }
    return arr;
  }, []);
}
```

Chúng ta chạy thử và in kết quả dưới dạng một chuỗi JSON để thấy rõ cấu trúc dạng cây:

```
let list = require('./menu.json');
let result = buildTree(list);
console.log(JSON.stringify(result, null, '  '));
```

<details>
<summary>Nội dung JSON của kết quả thu được</summary>
<pre><code class="hljs javascript">[
  {
    "name": "Cars",
    "items": [
      {
        "name": "SUV",
        "items": [
          {
            "name": "Honda",
            "items": []
          },
          {
            "name": "Ferrari",
            "items": []
          }
        ]
      },
      {
        "name": "Sedan",
        "items": []
      },
      {
        "name": "Sports",
        "items": []
      }
    ]
  },
  {
    "name": "Electronics",
    "items": [
      {
        "name": "Laptop",
        "items": [
          {
            "name": "Thinkpad",
            "items": []
          }
        ]
      },
      {
        "name": "Headphones",
        "items": [
          {
            "name": "Shure",
            "items": []
          },
          {
            "name": "Bose",
            "items": []
          },
          {
            "name": "JLB",
            "items": []
          }
        ]
      }
    ]
  }
]
</code>
</pre>
</details>

## Phức tạp quá, làm vậy chi cho cực?

Đọc đến đây hẳn có bạn sẽ đặt ra câu hỏi: Tại sao ngay từ ban đầu lại dùng kiểu dữ liệu dạng như thế này trên frontend? Chẳng phải đây là việc của backend hay sao? Trên thực tế có ai làm vậy không?

Câu trả lời nằm ở tính tiện lợi khi cần mở rộng menu dạng cây như thế này, ví dụ, với mảng $I$ đã cho ở trên, khi chúng ta cần mở rộng, ví dụ thêm vào một loại laptop khác bên cạnh dòng **Thinkpad**, hoặc thêm vào một hãng xe ô tô mới, ta có thể thêm trực tiếp một hoặc nhiều item mới vào mảng $I$, ví dụ:

**menu.json**
```javascript
[
  ...,
  {
    "name": "Laptop",
    "items": ["Thinkpad"]
  },
  {
    "name": "Thinkpad",
    "items": ["X220", "T450"]
  },
  {
    "name": "X220",
    "items": ["New", "Second Hand"]
  },
  ...
]
```

Nếu lưu dữ liệu trực tiếp dạng cây thì việc cả việc lưu trữ lẫn việc thêm/bớt này quả thực là một vấn đề không đơn giản. Hay nói cách khác thì giải pháp đó không scalable.

## Bài viết này có giúp tôi được tăng lương không?

**Lưu ý:** Bản quyền câu nói trên thuộc về blog [Quần Cam](https://quan-cam.com), để đưa câu nói này vào bài viết, mình đã phải trả một khoản "tiền" khá lớn, mong các bạn đừng bắt chước.

Chân thành mà nói thì việc tăng lương hay không phụ thuộc vào... sếp của bạn :joy:

Tuy nhiên, là một frontend developer, bạn cũng là một phần rất quan trọng trong team, nếu phải lựa chọn giữa một giải pháp **tiện lợi cho cá nhân mình nhưng sẽ tạo ra gánh nặng cho phần còn lại của team**, và một giải pháp **tạo ra vài thách thức nhỏ cho bản thân nhưng đem lại hiệu quả cho toàn bộ hệ thống**, thì có lẽ hy sinh một chút thời gian và sự thoải mái của bản thân sẽ tốt hơn so với việc ngồi câu nệ xem việc này là của ai và ai phải làm gì.

Rất cảm ơn các bạn đã đọc đến tận dòng này, hy vọng các bạn thích sê ri này, đừng ngại để lại comment nếu các bạn có bất kỳ góp ý hay gợi ý gì. Hẹn gặp lại các bạn trong các bài viết sắp tới.
