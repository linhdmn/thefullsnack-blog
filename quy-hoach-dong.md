# Vài ghi chép về Quy hạc động

Vầng, thì là **Quy hoạch động**, không phải Quy hạc động, tiếng Tây họ gọi là **Dynamic Programming**, là cơn ác mộng của nhiều người, trong đó có tui.

Hồi xưa đi học chắc thầy cô có giảng, nhưng vì ko nghe giảng nên tới giờ vẫn còn lan man.

Vậy nên giờ phải tự học, mà tự học không có cách nào khác ngoài cách tìm đọc lại lý thuyết và làm bài tập.

---

## Các cách tiếp cận thường gặp

Đối với hầu hết các bài viết, sách vở nói về Quy hoạch động mà chúng ta có thể dễ dàng tìm thấy trên mạng, nhà sách,... thì đều có thể chia làm 2 dạng thường gặp:

### Dạng 1: Đưa ra ví dụ từ quá cơ bản đến quá cao.

Đối với dạng này, thường thì ví dụ đầu tiên sẽ bàn về cách giải bài toán sinh dãy Fibonacci bằng phương pháp đệ quy (top-down approach), sau đó minh họa cách tiếp cận bằng quy hoạch động (bottom-up approach). Sau đó sẽ nhảy ngay đến các dạng bài toán phức tạp hơn như Dãy con chung dài nhất (Longest Common Sequence),... đồng thời đưa ra ngay công thức truy hồi một cách rất bất ngờ và đầy tính ma thuật. Nếu ko có căn bản thì chắc chắn rằng bạn sẽ bị mất phương hướng ngay tại bước này. 

Nhược điểm của các tài liệu sử dụng phương pháp này là:

 - Ví dụ đầu tiên quá đơn giản để có thể nhận ra các tính chất của một bài toán của quy hoạch động
 - Ví dụ tiếp theo sau đó rất điển hình, nhưng cách phân tích và đưa ra lời giải quá magic, tin tưởng quá mức vào cái gọi là "kinh nghiệm giải bài" mà không đưa ra một cơ sở suy luận nào cả.

Có thể tìm thấy phương pháp này ở hầu hết các bài viết trên mạng, hoặc trong cuốn **Cracking the Coding Interview**.

Kinh nghiệm rút ra là không nên đọc các dạng tài liệu như thế này.

### Dạng 2: Đưa ra ví dụ quá phức tạp ngay từ đầu

Đối với dạng này thì các ví dụ được đưa ra ngay từ đầu bài rất bự, rất phức tạp và tốn nhiều thời gian để có thể đọc-hiểu-ngấm.

Ngoài cái rào cản ban đầu ra thì các dạng này đều có phân tích bài toán ngay từ đầu, đưa ra hướng giải từng bước nhưng để thực sự hiểu được thì bạn cần dành ra khá nhiều thời gian để kiểm chứng công thức mà tác giả đưa ra. Và thường chúng đều là các biểu thức toán học.

Ví dụ như vấn đề về **Dây chuyền sản xuất trong nhà máy** (Assembly line) của cuốn **Introduction to Algorithms**, với lối giải thích khá nặng về toán.

Hay cách phân tích kết hợp so sánh giữa các kĩ thuật theo từng dạng bài như trong cuốn **The Algorithm Design Manual**, ít toán hơn, có ví dụ bằng code, nhưng không quá dễ hiểu.

Và không thể không nhắc đến quyển sách **Giải thuật và Lập trình** của thầy Lê Minh Hoàng. Ưu điểm là giải thích rõ ràng, bằng tiếng Việt.

Đây là các dạng tài liệu nên đọc. Và cần dành nhiều thời gian mới đọc được.

---

## Các thắc mắc không biết hỏi ai

Nhìn chung thì dù là tài liệu dạng nào, đều sẽ xuất hiện cùng một vấn đề đó là: có những thời điểm mà người viết đưa ra một vài nhận định mà chúng ta không biết là từ đâu ra, hoặc phải thừa nhận là nó đúng, thường thì cái này đến từ kinh nghiệm hơn là lý luận.

Và nếu bạn là con người đa nghi giống như tui thì bạn sẽ đặt ra vô vàn câu hỏi tại sao:

 - Tại sao lại nghĩ ra được cách phân tích như vậy?
 - Tại công thức truy hồi lại viết như thế này, mà không viết như thế khác?
 - Có vẻ như để phân tích được bài toán và suy ra được công thức truy hồi thì cần rất nhiều kinh nghiệm, cho nên người ta thường khuyến khích làm bài tập nhiều vào là sẽ quen dạng toán? Nghe có vẻ phản khoa học.
 - Lỡ phân tích vấn đề sai thì coi như sai luôn cả bài?


