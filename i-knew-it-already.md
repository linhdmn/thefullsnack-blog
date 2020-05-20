# Hiểu và Biết

Đó là 2 mức độ khác biệt nhau trong việc học.

Nếu sắp xếp theo kiểu [mức độ dốt](http://www.procul.org/blog/2005/09/01/nnm-m%E1%BB%A9c-ngu-d%E1%BB%91t/) thì ta có thứ tự như này:

- Chưa biết
- Biết
- Hiểu

Từ hồi còn đi học cho đến khi đi làm và tiếp xúc với nhiều người, tham gia nhiều buổi tranh luận và phần lớn thời gian thì mình phát hiện ra, trừ những lúc mình ở vị trí _Hiểu_ hoặc _Chưa biết_, những lúc mình _Biết_ là những lúc mình hay cãi ngu nhất :pensive:.

---

**Chưa biết** là khi chúng ta tiếp cận với một vấn đề hoàn toàn mới mà ta chưa hề có bất kỳ một khái niệm hay manh mối gì về nó. Ở mức này, chúng ta dễ dàng vứt bỏ cái tôi ngu dốt của mình để học hỏi nó từ những người đi trước.

**Biết** là khi chúng ta đã có chút khái niệm, chút thông tin bề mặt và đã từng sử dụng qua thứ kiến thức mới đó. Ví dụ:

- Chúng ta biết khai báo `const char*` trong C, và biết cách dùng `strlen()` để tính độ dài của chuỗi đó. 
- Chúng ta biết dùng `async` và `await` để thực hiện các tác vụ bất đồng bộ trong JavaScript.
- Chúng ta biết GRUB là một bootloader, và bootloader thì có nhiệm vụ tiếp nhận quyền xử lý từ BIOS để chuyển giao nó cho hệ điều hành.
- Chúng ta biết bỏ tiền ra mua card đồ họa xịn để đào bitcoin

Để đạt tới mức độ **Biết** này, có thể chúng ta đã **được dạy sơ qua** hoặc đã **tự đọc/tự thực hành** về kiến thức này ở đâu đó, do đó, khi tình cờ gặp lại nó, chúng ta nhớ là đã từng tiếp xúc với nó, như vậy chúng ta nghĩ rằng mình đã biết về nó. Mà khi đã biết thì cần gì phải tốn thời gian cho nó lại nữa.

Nhưng khi đụng phải các trường hợp cần đi sâu vào những kiến thức đó, đa phần chùng ta sẽ trở nên cực kì lúng túng, ví dụ khi có ai đó hỏi những câu như:

- Tại sao phải dùng một hàm bên ngoài là `strlen()` để tìm độ dài của một chuỗi trong C?
- Tại sao nếu khai báo một chuỗi với cách `const char* s` thì chúng ta không thể gán một ký tự `s[i]` cho chuỗi `s` này được?
- Tại sao dùng `async` thì có thể chạy được một tác vụ tốn thời gian mà không làm cho trang web bị "đứng hình"?
- Tại sao code JS "nặng" lại làm trang web bị "đứng hình"?
- Bạn đã bao giờ nghe nói về chu kỳ update mỗi 16ms của trang web chưa?
- Tại sao `setTimeout(0)` không bao giờ thực thi ngay lập tức như lời quảng cáo?
- Event Loop của một tab trong trình duyệt là gì?
- BIOS thực sự giao quyền kiểm soát máy tính cho bootloader bằng cách nào?
- [Tại sao](http://archive.is/LzjcV) bootloader phải được nạp ở vị trí `0x7C00` trên đĩa?
- [Tại sao](https://thefullsnack.com/posts/nhan-ma-tran-2.html) phải dùng card đồ họa (GPU) để đào bitcoin?
- Tại sao không thể dùng CPU để đào?
- Tại sao người ta lại tin vào lời thầy bói?
- ...

Cái chúng ta quên mất đó là chúng ta chỉ mới **biết** cái phần **bề mặt** của một vấn đề. Và với nhiều người (trong đó từng có mình), biết cái **bề mặt** tức là đã biết hết.

You know nothing, Jon Snow!

Cái kiến thức đã **biết** này không thực sự hữu dụng. Để nó thực sự trở thành thứ kiến thức mà ta có thể kiểm soát được, ta cần phải **hiểu** nó.

Cách duy nhất để **hiểu** đó chính là đào sâu vào. Ví dụ:

- Muốn hiểu về chuỗi trong C, phải tìm hiểu sâu về cách mà C thể hiện một chuỗi trong bộ nhớ, về con trỏ và đọc về cách mà các thư viện chuẩn implement những hàm chúng ta thường dùng để xử lý chuỗi.
- Muốn hiểu về `async` `await` trong JavaScript, chúng ta nên lùi lại vài bước để tìm hiểu về [event loop](http://exploringjs.com/es6/ch_async.html) của trình duyệt.
- Muốn hiểu về cách mà bootloader hoạt động, chúng ta nên đọc về cách để viết một bootloader, hoặc đọc source của một hệ điều hành nào đó, ví dụ như linux kernel phiên bản đầu tiên. 
- Muốn hiểu tại sao phải đào bitcoin bằng GPU, chúng ta nên đọc về blockchain, proof-of-work,...
