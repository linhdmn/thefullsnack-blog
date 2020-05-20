---
title: Kinh nghiệm làm việc với Cgo
published: true
date: 2016-07-11 00:16:11
tags: code, c, go
description: Cuối tuần vừa rồi mình có làm một project nho nhỏ để học hỏi thêm, điểm khá thú vị là project này tuy không to lắm nhưng lại chứa khá là nhiều vấn đề và buộc mình phải tìm hiểu sâu vào rất nhiều mảng kĩ thuật.
image:
---
Cuối tuần vừa rồi mình có làm một project nho nhỏ để học hỏi thêm, điểm khá thú vị là project này tuy không to lắm nhưng lại chứa khá là nhiều vấn đề và buộc mình phải tìm hiểu sâu vào rất nhiều mảng kĩ thuật.

Một trong những chức năng của project này là việc chụp ảnh màn hình (screenshot), cũng nhờ vậy mà biết được việc chụp ảnh màn hình trên từng hệ điều hành khác nhau là một công việc không hề đơn giản và chắc chắn không thể thực hiện trực tiếp trong Golang mà phải thông qua API của từng hệ điều hành, chính vì vậy nên cần phải đụng tới Cgo. Chi tiết về việc chụp ảnh chắc mình sẽ viết ở một bài viết khác, bài này mình chia sẽ một vài điều rút ra khi làm việc với Cgo trong 2 ngày cuối tuần vừa rồi.

## Giới thiệu nhanh về Cgo 

Cgo tức là C kết hợp với Go, nói đơn giản thì Cgo giúp cho Go có thể gọi được code C và ngược lại, gọi code C trong Go... à nhầm, gọi code Go trong C.

Để gọi được code C trong Go thì chúng ta viết đoạn code C đó trong một khối comment `/* */` hoặc `//` rồi import gói `"C"`. Trình biên dịch sẽ tự động chia ra, gọi `gcc` (hoặc trình biên dịch C default của máy tính) để biên dịch đoạn code trên, sau đó go compiler sẽ làm nhiệm vụ liên kết nó vào chương trình.

Chúng ta có thể truy cập vào C code thông qua đối tượng `C` trong Go, ví dụ:

```
package main

/*
#include <stdio.h>

void sayHello() {
  printf("YOLO!")
}
*/
import "C"

func main() {
  C.sayHello()
}
```

Nếu muốn tìm hiểu chi tiết hơn về Cgo các bạn có thể tham khảo các link ở cuối bài.

Sau đây mình sẽ nói sơ về một vài kinh nghiệm mình gặp phải trong quá trình làm việc với nó.

## Cygwin không tương thích với Cgo

Nếu dùng Windows hẳn các bạn sẽ dùng Cygwin, và trình biên dịch `gcc` đi kèm của Cygwin thì không có khả năng biên dịch được Cgo.

Để làm việc với Cgo trên Windows bạn bắt buộc phải sử dụng Command Line của win, và biên dịch bằng MingGW.

## Cgo không chỉ có C

Nếu làm việc trên Mac, bạn còn có thể dùng Cgo để gọi code Objective-C và xài các framework của Objective-C như Cocoa ngay trong Go luôn cũng được, ví dụ:

```
/*
#cgo CFLAGS: -x objective-c
#cgo LDFLAGS: -framework Cocoa -framework Foundation
#import <Cocoa/Cocoa.h>
#import <Foundation/Foundation.h>

...
*/
```

## Memory Leak từ phía C

Go có garbage collector (GC), và GC của Go hoạt động trong lúc runtime luôn. Nhưng C thì không như vậy. Điều này có nghĩa là khi làm khởi tạo các object bên trong phần C, bạn phải quản lý chúng bằng tay (tạo ra thì nhớ hủy nó đi), nếu không thì memleak sẽ là một vấn đề đau đầu. 

Cách đơn giản nhất để phát hiện memleak là dùng Task Manager của Windows hoặc Activity Monitor của Mac theo dõi bộ nhớ sử dụng của chương trình, nếu code logic của bạn không làm gì nhiều nhưng memory cứ tăng lên liên tục, thì khả năng đó là nó đang bị leak.

## Thời gian build chậm hơn

Rõ ràng rồi, thay vì chỉ có Go compiler biên dịch chương trình của bạn, thì lần này trong build process phải chạy đến 2 compiler (GCC và Go compiler) biên dịch 2 lần code và link chúng lại với nhau.

## Vấn đề dependencies khi deploy

Nếu dùng Cgo, khi user cài đặt chương trình Go của bạn thì trên máy tính của họ ngoài các thư viện mà Go phụ thuộc ra, họ còn phải cài đặt thêm các thư viện mà phần code C của bạn cần dùng tới. Điều này sẽ khiến cho việc deployment tốn thêm vài bước nữa để đảm bảo chương trình chạy tốt.

Èo, tạm thời tuần này mình chỉ ghi ra được một vài ý đơn giản vậy, còn nhiều thứ nữa nhưng mà giờ chưa hệ thống lại để diễn đạt một cách gọn gàng đường, chắc phải hẹn tuần sau thôi :D Nhưng kết luận là: Cgo là một chức năng khá hay và quan trọng của Go, giúp mở rộng Go để có thể sử dụng sức mạnh của nhiều công nghệ khác, nhưng bất cứ thứ công nghệ nào cũng đều có 2 mặt, thứ gì càng hay thì càng cần phải tìm hiểu rõ về nó để biết khi nào cần xài, khi nào thì nên tránh. Đúng với câu nói vui của một anh bạn mình:

> Hiểu để xài nhau tốt hơn

Chốt bài viết mình xin dẫn một vài link nên đọc về Cgo cho bạn nào muốn tìm hiểu sâu thêm về nó:

 [[1] Go Blog - C? Go? Cgo!](https://blog.golang.org/c-go-cgo)
 
 [[2] Godoc - Command cgo](https://golang.org/cmd/cgo/)

 [[3] Wiki Cgo](https://github.com/golang/go/wiki/cgo)

 [[4] Blog của bác Dave Cheney - Cgo is not Go](http://dave.cheney.net/2016/01/18/cgo-is-not-go)

 [[5] Cockroach Labs - The Cost and Complexity of Cgo](https://www.cockroachlabs.com/blog/the-cost-and-complexity-of-cgo/)
