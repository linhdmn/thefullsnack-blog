# Đôi điều về Animation bằng CSS

Hẳn các bạn vẫn còn nhớ những ngày đầu làm quen với lập trình Web, và nghe đến khái niệm web động, người dạy/người viết sách luôn nhấn mạnh, web động là thể loại web có thể load và thay đổi được nội dung, chứ không phải là một trang web biết động đậy.

Trong bài viết này mình cũng sẽ nói về web động, nhưng mà là `web động đậy` (animation).

Bối cảnh hình thành bài viết này là cách đây mấy hôm mình có đi tìm hiểu về `động đậy` (animation) để chuẩn bị kiến thức đi phỏng vấn, bài viết này không thể không nhắc đến công sức của thím `An Cao (@kcjpop)`, đã cung cấp khá nhiều bài viết hay ho để mình có tư liệu nghiên cứu.

## Các cách động đậy

Với CSS, chúng ta có 2 cách để thực hiện animation cho một element, đó là:

- `transition`: là thuộc tính giúp kiểm soát thời gian thay đổi của các thuộc tính CSS trên một element, thường được dùng cho các hiệu ứng đơn giản, như thay đổi một hay một vài thuộc tính, từ trạng thái đầu sang trạng thái đích, ví dụ:
  ```css
  button {
    transition: opacity 0.5s;
    background-color: #F00;
    opacity: 1.0; /* trạng thái đầu */
  }

  button:hover {
    opacity: 0.5; /* trạng thái đích */
  }
  ```
- `animation`: là thuộc tính giúp chúng ta áp dụng các chuyển động được định nghĩa sẵn bằng từ khóa `@keyframes` vào cho một element, thường được dùng cho các chuyển động có tính liên tục, lặp đi lặp lại và phức tạp, ví dụ:
  ```css
  @keyframes rainbow {
    0%  { background: #F00; }
    25% { background: #FF0; }
    50% { background: #F0F; }
    75% { background: #0FF; }
  }

  button {
    animation-name: rainbow;
    animation-duration: 0.5s;
    animation-iteration-count: infinite;
  }
  ```

## Động đậy với CSS transition

Như đã đề cập ở trên, chúng ta dùng `transition` khi muốn thực hiện các chuyển động nhỏ, từ trạng thái này sang trạng thái khác, ví dụ như đổi màu, đổi tọa độ, kích thước,...

Chúng ta có thể kiểm soát thời gian thực hiện việc thay đổi bằng thuộc tính `transition-duration`, giá trị nhập vào có đơn vị tính bằng giây. Chúng ta cũng có thể chỉ định thời gian chờ (delay) trước khi animation diễn ra bằng thuộc tính `transition-delay`.

Nhưng một điểm cần lưu ý đó là các `transition` không thể lặp (loop) được <ref>1</ref>. Cho nên đây cũng là yếu tố để cân nhắc khi nào nên dùng `animation` và khi nào dùng `transition`.

Để chỉ định thuộc tính CSS nào được animated, ta dùng `transition-property`, lưu ý là không phải thuộc tính nào cũng có thể animate được, các bạn có thể xem thêm [danh sách sách các thuộc tính animatable tại đây](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties), nếu lười thì cứ để thuộc tính trên là `all` để nó chạy được cái nào thì chạy.

Nếu không thích viết quá nhiều dòng để thiết lập `transition` thì ta có thể viết ngắn gọn lại như sau:

```css
transition: <thuộc tính> <thời gian> <timing-function> <delay>;
```

Cái hàm `timing-function`, nếu các bạn chăm chỉ đọc bài của mình :smirk: thì sẽ nhớ mình đã từng đề cập đến nó một lần trong bài [mô phỏng Facebook Reaction](https://thefullsnack.com/posts/phuc-tap-hoa-facebook-reaction.html), sử dụng timing function cũng là cách duy nhất làm cho hiệu ứng animation bằng `transition` bớt đơn giản hơn.

Một điểm nữa cần lưu ý đó là `transition` không được kích hoạt tự động (ví dụ lúc page load), mà chỉ bắt đầu animate khi có sự thay đổi nào đó (một thuộc tính thay đổi, hoặc một class được thêm vào/xóa ra dẫn tới các thuộc tính thay đổi,...).

Những cách để kích hoạt `transition` thường gặp là dùng JavaScript add thêm class, hoặc dùng luôn pseudo class như `:hover`.


## Động đậy với CSS animation



---

**Notes**

- <cite id="1">Thực ra `transition` vẫn có thể lặp được bằng cách sử dụng JavaScript để bắt sự kiện `transitionend`, rồi xử lý tiếp, nhưng cách thực hiện như vậy là quá phức tạp, không cần thiết khi chúng ta vẫn có giải pháp thay thế.</cite>
