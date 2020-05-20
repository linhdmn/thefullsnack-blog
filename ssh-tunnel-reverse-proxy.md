---
title: Tự tạo SSH tunnel để forward port ra remote server
published: true
date: 2016-09-12 11:01:26
tags: random
description: Khi làm việc, đôi lúc bạn chạy server ở localhost nhưng cần truy cập vào nó từ một thiết bị khác không cùng trong mạng nội bộ, ví dụ chạy web để demo cho khách hàng xem, hoặc muốn test API trên localhost của bạn từ mobile app.
image:
---
Khi làm việc, đôi lúc bạn chạy server ở localhost nhưng cần truy cập vào nó từ một thiết bị khác không cùng trong mạng nội bộ, ví dụ chạy web để demo cho khách hàng xem, hoặc muốn test API trên localhost của bạn từ mobile app.

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/case1.png_b3vpziib2q)

Khi đó bạn cần phải forward port mà server app của bạn đang sử dụng (localhost) ra một server bên ngoài (remote server) để người khác có thể truy cập vào. 

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/case2.png_jqff6yueh0)

Để làm điều này thì bạn có thể sử dụng các dịch vụ như [ngrok](https://ngrok.com/), [PageKite](https://pagekite.net/), [Forward](https://forwardhq.com/).

Nhưng nếu có một server riêng thì chúng ta hoàn toàn có thể tự setup một dịch vụ như vậy để xài riêng mà không cần phải phụ thuộc vào ai. Bằng cách sử dụng SSH tunnel.

## Setup từ phía server

Yêu cầu đầu tiên là server phải được setup để truy cập được từ bên ngoài (giả sử có domain là `kipalol.com`) và phải hỗ trợ SSH.

Truy cập vào server và thêm dòng cấu hình `GatewayPorts` vào file `/etc/ssh/sshd_config` như sau:

```
GatewayPorts yes
```

Sau đó thì restart lại SSH server:

```
service ssh restart
```

## Tạo tunnel từ phía localhost

Giả sử trên localhost của bạn đang chạy một Rails app ở cổng `3000`, và bạn muốn mọi người có thể truy cập vào Rails app này thông qua cổng `9000` từ phía server của bạn. Ví dụ: http://kipalol.com:9000

Ta chạy lệnh sau trên máy localhost:

```
ssh -N -R 9000:localhost:3000 user@kipalol.com
```

Các tham số:

- `-N` để đảm bảo là bạn không kết nối ở chế độ login vào SSH server
- `-R` để tạo tunnel tới server
- `9000` là cổng ở server mà bạn muốn mở để truy cập vào Rails app ở localhost
- `localhost:3000` là địa chỉ truy cập và cổng của Rails app
- `user@kipalol.com` là user và địa chỉ truy cập mà bạn dùng để SSH vào server 

Từ bây giờ, user có thể truy cập vào http://kipalol.com:9000 để truy cập trực tiếp vào Rails app mà bạn đang chạy trên máy cá nhân của mình. 

Ngoài ra các bạn có thể tìm hiểu thêm về các ứng dụng khác của SSH tunnel qua [bài này](http://kipalog.com/posts/Gioi-thieu-SSH-Tunnel-va-mot-so-ung-dung) 
