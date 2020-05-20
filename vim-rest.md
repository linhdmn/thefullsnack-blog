---
title: Dùng Vim làm REST API client
published: true
date: 2017-01-19 15:07:29
tags: vim
description: Thường thì khi cần test API, chúng ta sẽ dùng các HTTP client như là Postman, tuy nhiên cái này có một nhược điểm đó là nó... xài GUI. 
image:
---
Thường thì khi cần test API, chúng ta sẽ dùng các HTTP client như là Postman, tuy nhiên cái này có một nhược điểm đó là nó... xài GUI. 

Và một Vim user chân chính sẽ rất là không thoải mái nếu cứ phải nhảy qua nhảy lại giữa màn hình terminal và một ứng dụng GUI củ chuối nào đó.

Một giải pháp để bạn vẫn được yên phận với màn hình terminal đen ngòm đó là dùng `curl`.

Tuy nhiên đời chỉ đẹp nếu ta xài `curl` với những request đơn giản, vài ba params nho nhỏ. Trong trường hợp một request có hàng chục parameters, hoặc truyền vào vài ba cái header mỗi cái dài hơn trăm kí tự thì chắc không ai đủ kiên nhẫn để xài `curl` nữa.

Trước thực trạng nhức nhối này thì mình tính viết một plugin cho Vim để xài, ý tưởng là viết sẵn cấu trúc của một request vào buffer của Vim, rồi bấm nút 1 phát cho nó tự gọi `curl` thay mình.

Rất may cho sếp của mình, vì mình đã tìm được một plugin người ta viết sẵn trên mạng, nên mình ko cần ngồi làm việc riêng trong giờ làm việc nữa.

Mà còn điều này nữa, thực ra nãy giờ bạn không cần phải đọc những gì mình viết trên kia đâu, vì đây mới bắt đầu phần chính nè :v 

## Plugin: Vim Rest Console

[vim-rest-console](https://github.com/diepm/vim-rest-console) là một plugin giúp gửi/nhận request tới RESTAPI server, rất tiện dụng để test ngay trong Vim.

Plugin này có thể xài thoải mái với Vim và Neovim, miễn là máy tính cần có `curl`.

### Cài đặt

Cài với các plugin manager như VimPlug hay Vundle:

```
Plug 'diepm/vim-rest-console'
```

Hoặc nếu bạn xài pathogen thì cài theo cách sau:

```
cd ~/.vim/bundle
git clone https://github.com/diepm/vim-rest-console.git
```

### Sử dụng

Bạn có thể mở vim ra và gõ ngay hoặc tạo một file có đuôi `.rest` để dùng hoặc tiện lưu lại về sau.

Nếu mở buffer trực tiếp thì có thể dùng lệnh sau để có syntax highlighting:

```
:set ft=rest
```

Sau đó gõ cấu trúc request theo dạng:

```
http(s)://<host>:<port>
<danh sách header nếu có>
<METHOD> <endpoint>
<request body>
```

Ví dụ:

```
https://facebook.com/api/v1
Content-Type: application/json
Pragma: no-cache
POST /delete_user/
{
  "username": "markzuck"
}
```

Nếu bạn muốn gửi một `POST` request đến `https://facebook.com/api/v1/delete_user` với request body là `username: markzuck`.

Sau khi gõ xong nội dung request, nhấm `Ctrl + J` để bắt đầu gửi request đi. 

Vim sẽ tạo ra một split mới để hiện kết quả request. Rất là tiện lợi.
