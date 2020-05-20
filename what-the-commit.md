---
title: Tiết kiệm thời gian commit bằng WhatTheCommit 
published: true
date: 2016-09-08 10:54:09
tags: git
description: 
image:
---
[WhatTheCommit](http://whatthecommit.com/index.txt) là một trang web cung cấp các đoạn text ngẫu nhiên, ý đồ của tác giả trang web này là để sử dụng nội dung này vào cho các commit khi làm việc với Git để tiết kiệm thời gian :)))

Chúng ta có thể dùng `curl` để lấy nội dung ngẫu nhiên từ trang web này về khi commit như sau:

```
git commit -m "$(curl -s whatthecommit.com/index.txt)"
```

Để tiết kiệm thời gian hơn nữa thì có thể tạo alias để thực hiện nhanh việc này:

```
alias yo='git add -A && git commit -m "$(curl -s whatthecommit.com/index.txt)"'
```
Ngoài ra thì mình còn tạo thêm một alias cho lệnh push như sau:

```
alias push="git push"
```

Vậy là mỗi khi cần push code nhanh thì chỉ việc gõ lệnh `yo` theo sau đó là lệnh `push`:

```
➜  notepad git:(master) ✗ yo
[master 9a2682a] Shit code!
 1 file changed, 1 insertion(+), 1 deletion(-)
➜  notepad git:(master) push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 289 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://<repo>
   540fa34..9a2682a  master -> master
➜  notepad git:(master)
```

