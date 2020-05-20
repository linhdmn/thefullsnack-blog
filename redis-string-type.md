# String của Redis

Có lẽ không ai lạ gì định nghĩa về string ở trong C, một string là:

- Một mảng gồm các kí tự kiểu `char`
- Kết thúc bằng một kí tự NULL `\0`
- Một biến kiểu string thực chất là một pointer trỏ tới vị trí đầu tiên của mảng các kí tự trong bộ nhớ

Vì là một pointer (con trỏ), chúng ta không có cách nào để biết được độ dài (length) của một string ngoài cách duyệt từ đầu đến cuối chuỗi và đếm. Ngay cả bộ thư viện chuẩn `string.h` của Glibc [cũng dùng cách này](https://github.com/lattera/glibc/blob/a2f34833b1042d5d8eeb263b4cf4caaea138c4ad/string/strlen.c#L26). Kiểu như này:

```c
do
  len = len + 1;
while (s[len] != '\0');
```

Hoặc:

```c
for(len = 0; s[len] != '\0'; ++len);
```

Thuật toán như trên có độ phức tạp $O(n)$, string càng dài, thời gian chúng ta tốn cho việc đếm độ dài càng lớn.

## Vấn đề của C string

Trong các thao tác xử lý chuỗi, việc tính length là một thao tấc được thực hiện gần như là thường xuyên, việc lặp liên tục như vậy quả là không hay ho gì cho lắm, nhất là với các string có độ dài lớn.

Đó là còn chưa kể tới một hiểm họa, nếu string đó không hợp lệ (không kết thúc bằng kí tự `\0`) thì coi như vòng lặp là vô hạn (thực ra là lặp cho tới khi biến `len` kia bị overflow).

Vậy cho nên mình khá là tò mò không biết trong những ứng dụng lớn cỡ như Redis hay Nginx thì người ta xử lý string thế nào, vì rõ ràng nếu xài thư viện chuẩn của C sẽ rất là không chuẩn, phải tốn thêm nhiều bước xử lý khác.

## Thư viện xử lý chuỗi của Redis

Trong source code của Redis, chúng ta sẽ thấy có 1 file tên là [sdc.c](https://github.com/antirez/redis/blob/unstable/src/sds.c), đây là thư viện xử lý string của Redis, được tác giả build sẵn thành một thư viện riêng nằm tại repo [antirez/sds](https://github.com/antirez/sds).

Cấu trúc của một string được tác giả define như sau:

```c
struct sdshdr {
    int len;
    int free;
    char buf[];
};
```

Thể hiện trên bộ nhớ theo sơ đồ sau:

```
+------------|------------------------|-----------|---------------\
| Len | Free | H E L L O W O R L D \n | Null term |  Free space   \
+------------|------------------------|-----------|---------------\
             |
             `-> Pointer returned to the user.
```

Độ dài của string được lưu vào đầu struct, như vậy tại bất kì thời điểm nào, mọi thao tác (kiểm tra, update độ dài,...) cũng sẽ chỉ tốn $O(1)$. Rất thuận tiện.

Ngoài ra có một điểm khá hay, đó là để tương thích với các thư viện C khác, thư viện này chỉ trả về một con trỏ đến vị trí của `buf`, như vậy, khi sử dụng với các thư viện C khác, thì SDS cũng sẽ giống như một string bình thưòng.
