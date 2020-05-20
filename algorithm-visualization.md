---
title: Algorithm Visualization
published: true
date: 2017-12-06 15:01:47
tags: code, algorithm, visualization, c
description: 
image:
---
Algorithm Visualization là kĩ thuật hình tượng hóa quá trình hoạt động của một thuật toán, chúng ta thường thực hiện nó bằng nhiều cách khác nhau như: viết, vẽ, lập bảng giá trị,...

Tuần vừa rồi mình có làm một project nho nhỏ để giải trí, vừa để thực hành xem cách mà người ta thực hiện việc visualization nó như thế nào. Mình sử dụng phương pháp được giới thiệu bởi Chris Wellons qua bài viết [Inspecting C's qsort Through Animation](http://nullprogram.com/blog/2016/09/05/).

Các bạn có thể tham khảo source code tại đây: https://github.com/huytd/bubble-sort-visualized

Ý tưởng cơ bản sẽ là: **In trạng thái của input ra sau mỗi bước của thuật toán.** 

In bằng cách nào thì còn tùy thuộc vào kết quả mà bạn muốn thể hiện, ví dụ ở đây mình muốn tạo ra một ảnh GIF động, nên mỗi một bước của thuật toán, mình sẽ in ra input dưới dạng một frame của tấm ảnh GIF ấy.

Sản phẩm thu được sẽ như thế này:

![](img/bubble-sort-visualized.gif)

Mỗi frame được in ra dưới [định dạng PPM](http://netpbm.sourceforge.net/doc/ppm.html), có dạng, hàm [`draw_frame()`](https://github.com/huytd/bubble-sort-visualized/blob/master/bubble.c#L18) dùng để thực hiện việc này.

Thuật toán sử dụng để minh họa là Bubble Sort và được implement thông qua 2 hàm [`swap()`](https://github.com/huytd/bubble-sort-visualized/blob/master/bubble.c#L47) và [`bubble_sort()`](https://github.com/huytd/bubble-sort-visualized/blob/master/bubble.c#L53), trong này sẽ thấy mỗi bước chúng ta gọi hàm `draw_frame()`.

Chỉ có một đoạn cuối hàm `main()` thì mình [chạy một vòng for](https://github.com/huytd/bubble-sort-visualized/blob/master/bubble.c#L77-L79) để add thêm vài frame, mục đích là để kéo dài thời gian cho dễ nhìn kết quả cuối cùng.

Cuối cùng, mình dùng [lệnh `convert` của ImageMagick](https://github.com/huytd/bubble-sort-visualized/blob/master/Makefile#L10) để chuyển đổi các frame định dạng PPM sang GIF.

Vì tính mình mau chán nên làm tới đây thì chán luôn rồi :pensive: đành open source để mọi người tham khảo, ai có hứng thú thì có thể nghiên cứu tiếp kĩ thuật này, đây là một hướng khá là hay ho :D
