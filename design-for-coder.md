# Coder học design

Trong vô số các lần tiếp xúc của mình với bạn bè cùng làm developer, chủ đề của câu chuyện luôn luôn có ít nhất một lần đề cập tới chuyện: Làm sao để học design?

Lý do tại sao các coder lại muốn học design? Đơn giản là vì, coder thường hay có rất nhiều ý tưởng làm app hoặc làm web hoặc làm game, nhưng khổ một cái rất khó để kiếm được một anh bạn designer hợp ý và sẵn sàng làm không công =))) cho nên giải pháp tốt nhất là tự học design để hai tay hai súng, đánh đông dẹp bắc cho khỏe, khỏi cần nhờ vả.

Nhưng khổ một cái là rất ít người thành công trong việc tự học design, lý do thì rất nhiều: Không biết dùng Photoshop, vẽ mãi mà nó không đẹp lên được, không có mắt thẫm mỹ, không biết cách phối màu sao cho hợp lý,... hoặc nặng hơn nữa là: Không biết phải bắt đầu design từ đâu :)) những lý do đó tạo ra rào cản và dần dần làm cho chúng ta nản rồi bỏ cuộc hết.

Trong bài này mình sẽ giới thiệu từng bước để các bạn làm quen với việc design ở một mức độ đơn giản nhất, đủ để ứng dụng vào trong việc thiết kế các giao diện nhỏ gọn, vừa đủ xài đối với một coder, còn để trở thành một designer chuyên nghiệp đủ sức giành hết công việc của các bạn designer chân chính thì mình không dám hứa đâu nhé :))

## Design cái gì?

Mình thích bay nhảy, nên chúng ta sẽ design một app quản lý lịch bay đơn giản có cả giao diện web lẫn giao diện mobile.

App sẽ có các chức năng sau đây:

- Tạo thông tin chuyến bay
- Quản lý danh sách các chuyến bay
- Gửi thông tin (các) chuyến bay đến cho người khác

Vậy là xong phần idea. Bây giờ chúng ta đi vào tiết mục chính.

## Bước 0: Ăn cắp ý tưởng

Xin nhấn mạnh một lần nữa ăn cắp là rất xấu, và nếu các bạn đã có ý tưởng trong đầu rằng mình sẽ design cái app như thế nào, cần hiển thị những thông tin gì,... các bạn nên bỏ qua bước này. 

Nhưng nếu các bạn chưa biết phải bắt đầu từ đâu, thì chúng ta có thể "nghía" qua các mẫu design hiện có và được sử dụng phổ biến một chút, để xem thiên hạ họ làm như thế nào, chỉ để có chút manh mối và hướng đi thôi, nhớ nhé, ăn cắp là xấu lắm đấy, cho nên phải ăn cắp một cách thông minh vào :)))

Một trong những site thích hợp để "ăn cắp" ý tưởng là [Pttrns](http://pttrns.com/) và [Dribbble](https://dribbble.com/)

Hãy search qua một vòng những từ khóa liên quan đến app, ví dụ như: **Ticket**, **Boarding Pass**, **Flights**, **Travel**, **Flight Planning**,... để xem một app liên quan đến lập lịch bay thì sẽ có những điểm gì cần thiết.

Sau một vòng nghiên cứu, hẳn các bạn cũng sẽ nhận ra, một app hiển thị lịch bay thì sẽ cần có các thông tin cần thiết sau đây:

- Sân bay xuất phát (Departure Airport)
- Sân bay đến (Arrival Airport)
- Ngày giờ xuất phát (Departure Time)
- Ngày giờ đến (Arrival Time)
- Tên chuyến bay (Flight Number)
- Mã số xác nhận để CheckIn (Confirmation Number) 

Bên cạnh những thông tin này, chúng ta cũng có thể thêm thắt một vài thông tin để app trở nên hữu dụng hơn, ví dụ:

- Ga / Cổng xuất phát (Departure Terminal / Gate)
- Ga / Cổng đến (Arrival Terminal / Gate) 
- Danh sách hành khách (Passengers)
- Số ghế ngồi (Seat)
- Thời gian bay (Flight Duration)
- Thời gian còn còn lại (Remaining Time)
- Số trạm dừng (Stops)
- Mã vạch - nếu có (Barcode)
- ...

Hẳn đến đây sẽ có bạn đặt ra câu hỏi, tại sao phải phân chia ra những thông tin nào cần thiết và thông tin nào thì chỉ là phụ thêm, tại sao không hiển thị hết luôn?

Câu trả lời là: Sẽ chẳng có ai muốn xài một app hiển thị quá nhiều thông tin nếu họ không thực sự cần chúng, và hiển thị quá nhiều thông tin chỉ góp phần làm cho người dùng cảm thấy rối rắm, dễ bị quên mất những thông tin quan trọng và giao diện của app sẽ trở nên cực kì tệ.



