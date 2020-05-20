---
title: Quick and Dirty Stack, Queue and Deque in JavaScript
published: true
date: 2018-01-16 14:14:51
tags: code, javascript, algorithm
description: 
image:
---
Trong quá trình phỏng vấn, dùng JavaScript, nếu đề bài không yêu cầu bắt buộc phải implement Stack hoặc Queue thì chúng ta có thể tiết kiệm thời gian bằng cách sử dụng Array.

Sở dĩ tiêu đề nói Quick and Dirty là vì cách làm này có thể không bảo đảm đúng hoàn toàn về time complexity, nhưng bù lại nó đúng về mặt concept và behavior của các kiểu dữ liệu, giúp tiết kiệm thời gian khi làm bài phỏng vấn (nhưng đồng thời bạn cũng phải nói rõ vấn đề này với người phỏng vấn, nếu không thì ăn hành ngay).

Việc khai báo Array trong JavaScript khá đơn giản:

```javascript
// Khai báo mảng rỗng
let array = [];
// Hoặc khai báo mảng n phần tử, fill nó bằng các giá trị 0
let array = Array.from(Array(n)).map(x => 0);
```

Trong JavaScript thì Array cũng được hỗ trợ một số thao tác, mà ta sẽ dùng nó để mô phỏng lại các kiểu dữ liệu đã đề cập, đó là:

- `push`: thêm một phần tử vào cuối array
- `pop`: lấy ra phần tử cuối cùng của array
- `unshift`: thêm một phần tử vào đầu array
- `shift`: lấy ra phần tử đầu tiên của array

Từ đây, ta có thể mô phỏng lại behavior của các kiểu dữ liệu thường gặp.

## Mô phỏng Stack

Hai thao tác chính trên stack đó là `push` và `pop`, hai hàm này có sẵn trong Array của JavaScript:

```javascript
let stack = [];
stack.push(5);
stack.pop();
```

Một thao tác cũng thường gặp trong các implementation của stack đó là `peek`, giúp xem trước giá trị nằm trên cùng của stack mà không cần lấy nó ra, ta có thể thực hiện việc này bằng cách truy cập trực tiếp dùng index:

```javascript
let last = stack[stack.length - 1];
```

## Mô phỏng Queue

Hai thao tác thường gặp của queue là `enqueue` (thêm một phần tử vào queue), và `dequeue` (lấy một phần tử ra khỏi queue), ta có thể thực hiện việc này bằng hai cách:

```javascript
let queue = [];
// Enqueue
queue.unshift(5);
// Dequeue
queue.pop();
```

Hoặc:

```javascript
let queue = [];
// Enqueue
queue.push(5);
// Dequeue
queue.shift();
```

Và bạn có thể thấy hai cách trên cũng thể hiện hai hướng hoạt động của một queue (từ trái qua phải, và từ phải qua trái), và nếu gộp chung cả hai cách thì nó cũng chính là một deque (double-ended queue).

Thao tác `peek` cũng tương tự như với stack, đặc biệt nếu là deque, thì ta sẽ có `peek_first` (để xem phần tử đầu), và `peek_last` (để xem phần tử cuối):

```javascript
let first = queue[0];
let last = queue[queue.length - 1];
```

## Độ phức tạp

Như đã nói ở đầu bài, mình không chắc về độ phức tạp của các thao tác trên nếu dùng Array, nhưng chắc chắn nó sẽ không đúng với độ phức tạp của các thao tác của stack và queue.

Ví dụ như hàm `shift`, trên queue nó chỉ là `O(1)`, tuy nhiên với JavaScript thì có lẽ nó sẽ chậm hơn rất nhiều.

Để hiểu rõ thêm cách mà JavaScript implement các hàm trên, bạn có thể xem qua đặc tả ECMAScript:

- [Array.prototype.push()](https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.push)
- [Array.prototype.pop()](https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.pop)
- [Array.prototype.shift()](https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.shift)
- [Array.prototype.unshift()](https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.unshift)

## Ứng dụng

Bây giờ mình sẽ thử một bài đơn giản, là implement thuật toán BFS (breadth-first search) và DFS (depth-first search) để duyệt một cây nhị phân, bằng cách sử dụng Array để mô phỏng stack và queue cho từng trường hợp.

Một node của cây có cấu trúc như này:

```javascript
function Node(val) {
    this.val = val;
    this.left = this.right = null;
}
```

Chúng ta sử dụng queue để implement thuật toán BFS, sau khi duyệt trả về một mảng các giá trị đã duyệt:

```javascript
const BFS = (root) => {
  let queue = [root];
  let result = [];
  while (queue.length) {
    let node = queue.shift();
    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
    result.push(node.val);
  }
  return result;
};
```

Để implement DFS thì ta dùng stack:

```javascript
const DFS = (root) => {
  let stack = [root];
  let arr = [];
  while (stack.length) {
    let node = stack.pop();
    if (node.left) stack.push(node.left);
    if (node.right) stack.push(node.right);
    arr.push(node.val);
  }
  return arr;
};
```

Giả sử với cây input như sau:

```
        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7
```

Kết quả của hai phép duyệt sẽ là:

```javascript
BFS(input) = [ 1, 2, 3, 4, 5, 6, 7 ]
DFS(input) = [ 1, 3, 6, 5, 7, 2, 4 ]
```

Happy Coding ^^
