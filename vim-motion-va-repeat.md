# Motion và Repeat trong Vim

Ở [bài trước](https://thefullsnack.com/posts/vai-phim-tat-vim.html) mình có đề cập tới **Visual mode** của Vim, để thực hiện các thao tác bôi chọn một vùng nội dung rồi thực hiện một thao tác nào đó. Đối với những ai vừa mới chuyển qua Vim từ các editor khác thì có thể sử dụng visual mode để làm quen dần dần, nhưng nếu xác định xài Vim lâu dài thì nên làm quen với khái niệm **Motion**.

## Motion

Motion, nôm na là thực hiện các thao tác bằng các phím điều hướng **(h, j, k, l)**

Ví dụ, ta kết hợp các phím điều hướng và phím số để:

- Dịch chuyển con trỏ qua bên trái 5 kí tự: **5h**
- Dịch chuyển con trỏ xuống 10 dòng: **10j**
- Dịch chuyển con trỏ qua phải 3 từ: **3w**

Nếu sử dụng thành thạo Motion, chúng ta có thể làm nhiều thao tác phức tạp hơn, ví dụ:

- Copy 5 dòng tiếp theo kể từ vị trí con trỏ hiện tại: **y5j**
- Delete 10 dòng tính từ vị trí con trỏ trở lên: **d10k**
- Tự động canh lề cho 50 dòng tiếp theo: **=50j**

Đọc qua thì có vẻ phức tạp nhưng nếu dùng thì sẽ thấy sự tiện lợi của nó.

## Relative Line Number

Để giúp cho việc đếm số dòng khi sử dụng Motion được thuận tiện hơn, Vim cung cấp một chức năng gọi là **Relavie Line Number**. Để kích hoạt tính năng này, thêm dòng sau vào `.vimrc` của bạn:

```
set relativenumber
```

Kết quả sẽ được như hình sau:

![Screen Shot 2016-07-29 at 10.01.46 PM.png](https://qiita-image-store.s3.amazonaws.com/0/110308/93d9d7bd-cd56-744f-2db9-caaada4a8a5d.png "Screen Shot 2016-07-29 at 10.01.46 PM.png")

Như hình trên thì vị trí con trỏ đang nằm ở dòng thứ 23, và các dòng phía trên, phía dưới của nó được đánh số theo khoảng cách từ dòng đó đến dòng hiện tại.

Giả sử mình muốn xóa toàn bộ nội dung của hàm `pixel()` thì chỉ cần gõ **d1k** ở dòng 23.

## Repeat

Một chức năng quan trọng khác của Vim đó là **Repeat**, với phím tắt là dấu ** chấm (.)**.

Khi nhấn nút repeat thì Vim sẽ lặp lại thao tác trước đó mà bạn vừa thực hiện.

Ví dụ:

Bạn có một đoạn nội dung như sau

```
The quick brown fox jumps over the lazy dog
```

Bạn muốn xóa cụm từ "quick brown fox jumps over the", thì bạn có thể sử dụng lệnh repeat kết hợp với lệnh xóa từ **dw** như sau:

- Đưa con trỏ đến đầu từ "quick"
- Xóa từ đó bằng lệnh **dw**
- Nhấn nút dấu chấm 5 lần

Và thế là cụm từ trên đã được xóa. Ở đây bạn cũng có thể kết hợp với motion để tăng tốc, ở bước cuối cùng thay vì nhấn nút dấu chấm 5 lần thì bạn chỉ cần gõ: **5.** (số 5 và dấu chấm), tức là lặp lại thao tác xóa vừa nãy 5 lần.

---

Nếu đọc xong mà cảm thấy Vim sao mà rắc rối quá, thì ko sao đâu, bản chất của Vim là sự kết hợp của nhiều phím tắt mà. Nhưng lợi ích mà nó đem lại thì không có editor nào có thể sánh bằng, bạn có thể thực hiện mọi thao tác edit chỉ bằng các tổ hợp trên bàn phím, không cần phải nhấc tay ra để đụng vào con chuột luôn.

Và như mọi khi, là lời khuyên: "Learn as you go", đừng bao giờ cố học thuộc các phím tắt, mà hãy nhớ bằng cách thực sự áp dụng nó khi bạn làm việc.

Happy vimming ^^ 
