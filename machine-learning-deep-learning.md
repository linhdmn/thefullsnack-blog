---
title: Deep Learning? Machine Learning? Học như thế nào?
published: true
date: 2016-10-23 21:08:11
tags: opinion, random, review
description: Mình thấy nhiều bạn bây giờ cứ muốn học là đâm đầu vào học Deep Learning, hoặc tuyên bố là sẽ học Deep Learning, trong khi chắc kí hiệu này $\theta$ chưa chắc đã biết đọc tên như thế nào... mình cảm thấy quan ngại sâu sắc :v 
image:
---
Có một sự thật phũ phàng là cái ngành khoa học xuất hiện từ những năm 1959 không ai quan tâm giờ lại được bà con đổ xô đi học nhờ ơn của báo giới lúc nào cũng ra rả về Deep Learning này Deep Learning nọ...

Mình thấy nhiều bạn bây giờ cứ muốn học là đâm đầu vào học Deep Learning, hoặc tuyên bố là sẽ học Deep Learning, trong khi chắc kí hiệu này $\theta$ chưa chắc đã biết đọc tên như thế nào... mình cảm thấy quan ngại sâu sắc :v 

Deep Learning chỉ là 1 mảng con của Machine Learning, và để tiếp cận được Deep Learning thì cần phải nắm được rất nhiều khái niệm cơ bản từ Machine Learning. Nếu không thì trong quá trình học các bạn sẽ rất dễ bỏ qua một vài keyword quan trọng, dẫn tới ko hiểu gì hết luôn.

Sau gần 1 năm theo đuổi việc tự học Machine Learning và thử nghiệm hết các thể loại course online, mình trích lược lại thành lộ trình học như sau, hy vọng sẽ giúp ích được cho nhiều bạn đang quan tâm và muốn bắt đầu tìm hiểu:

## Học lý thuyết

**Đầu tiên:** Học course ML trên Coursera của Andrew Ng kết hợp đọc tài liệu course [CS299](http://cs229.stanford.edu/) (đây là course chính của Andrew, cái course trên Coursera chỉ là trích lược từ course này)

**Tiếp theo:** Học tiếp course [Deep Learning của Google](https://www.udacity.com/course/deep-learning--ud730), và nếu muốn đi sâu vào Neural Network thì đọc trước bài [Neural Network And Deep Learning](http://neuralnetworksanddeeplearning.com/chap1.html) để nắm kiến thức cơ bản về Neural Network.

Để học được course này bạn cần có kiến thức về (trích nguyên văn trên udacity luôn nha):

```
- Basic machine learning knowledge (especially supervised learning)
- Basic statistics knowledge (mean, variance, standard deviation, etc.)
- Linear algebra (vectors, matrices, etc.)
- Calculus (differentiation, integration, partial derivatives, etc.)
```

Và 4 cái trên nó được cover trong course CS299.

**Sau cùng:**  Học tiếp course [CS231n](http://cs231n.stanford.edu/), vì course này chỉ nói về CNN dùng cho nhận diện hình ảnh (tức là một ngách hẹp hơn nữa của Neural Network)

## Học thực hành

Tất nhiên các khóa trên đều thuộc phạm trù Machine Learning Lý Thuyết (theo như phân loại trên [lộ trình học Machine Learning](https://daynhauhoc.com/t/lo-trinh-hoc-machine-learning-deep-learning-tu-dau-cho-cac-ban-lap-trinh-vien/37264) của a [@ZuzooVn](https://github.com/zuzoovn)), trong thời gian ôn luyện lý thuyết, bạn có thể order vài cuốn sách thiên về Machine Learning thực hành để làm quen ví dụ như cuốn:

- [Data Science from Scratch](https://www.amazon.com/Data-Science-Scratch-Principles-Python/dp/149190142X) 
- [Python Machine Learning](https://www.amazon.com/gp/product/1783555130/ref=pd_sim_14_2?ie=UTF8&psc=1&refRID=K6J013Z4FNHBS6S5YJ3Y)
- [Make your own Neural Network](https://www.amazon.com/Make-Your-Own-Neural-Network-ebook/dp/B01EER4Z4G/ref=sr_1_2?s=books&ie=UTF8&qid=1477850787&sr=1-2&keywords=neural+network)

Và học thêm [Sci-Kit](http://scikit-learn.org/stable/) hoặc TensorFlow qua [loạt bài ví dụ này](https://github.com/aymericdamien/TensorFlow-Examples) để tự implement thử vài thứ cho dễ hiểu thêm. Hoặc máu thì có thể tự implement thủ công hoàn toàn, trong trường hợp này mình khuyên nên dùng Swift hoặc Ruby vì cả 2 ngôn ngữ này đều có sẵn một vài cấu trúc dữ liệu cũng như hàm tính toán có thể vận dụng ngay, tránh tốn thời gian implement những cái vặt vãnh, để focus vào cái thuật toán toàn cục.

Hai bước thực hành và lý thuyết bạn có thể tiến hành song song, nhưng tuyệt đối ko đc bỏ phần lý thuyết nếu bạn muốn học ML một cách nghiêm túc :D
