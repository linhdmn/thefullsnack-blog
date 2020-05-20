---
title: Helm, Ripgrep và Emacs dùng... helm-ag
published: true
date: 2018-11-16 12:02:20
tags: emacs
description: Thủ thuật xài ripgrep với emacs
image:
---

Có một thủ thuật khá là tiện dụng khi dùng `helm` trong Emacs đó là lệnh `helm-resume` giúp khôi phục lại lệnh đã chạy trước đó, lệnh này đặc biệt hữu dụng khi đang search trong toàn project.

Lại nói về search, thường chúng ta hay xài `ag` (hay còn gọi là `the_silver_searcher`) vì nó có performance rất tốt, nhưng còn có một công cụ xịn hơn cả `ag` đó là `rg` (gọi là `ripgrep`, build bằng Rust, tất nhiên :joy:). Để tích hợp `ag` vào Emacs/Helm chúng ta có gói `helm-ag`, với `rg` thì chúng ta có `helm-rg`.

Nhưng `helm-rg` không play nice lắm với lệnh `helm-resume`, nó thường xảy ra lỗi đại loại là `helm-rg--make-process should run a process` sau khi resume, dẫn tới việc không jump tới buffer trong kết quả search được. Anh tác giả không chịu fix vì cho rằng đó là lỗi của `helm` hay `helm-projectile` gì đó, nói chung là đổ thừa, mà mình thì rất ghét dev có tính đổ thừa. Trong khi sự thật rõ rành rành, `helm-ag` vẫn chạy tốt khi resume! :angry:

Mà ghét rồi thì sao? Không lẽ bỏ không xài `rg` nữa, về xài `ag`? Đời nào lại thế?

Hóa ra, tác giả của `helm-ag` đã rất tinh ý khi hướng dẫn cả cách xài `rg` thay thế cho `ag`, documented ngay trong code của `helm-ag`! Đó là:

1. Vào màn hình tùy biến giá trị `M-x customize-set-variable`
2. Chọn giá trị `helm-ag-base-command`
3. Đặt giá trị mới thành: `rg --vimgrep --no-heading --smart-case`

Ba bước trên thì ai cũng biết rồi, không có gì đáng nói.

Cái đáng nói là khi kết hợp với `helm-projectile` (ai xài `projectile` thì sẽ gặp liền), `rg` sẽ phun ra một đống lỗi:

```
.#*: No such file or directory (os error 2)
*.o: No such file or directory (os error 2)
*~: No such file or directory (os error 2)
*.bin: No such file or directory (os error 2)
*.lbin: No such file or directory (os error 2)
*.so: No such file or directory (os error 2)
*.a: No such file or directory (os error 2)
*.ln: No such file or directory (os error 2)
*.blg: No such file or directory (os error 2)
*.bbl: No such file or directory (os error 2)
*.elc: No such file or directory (os error 2)
*.lof: No such file or directory (os error 2)
*.glo: No such file or directory (os error 2)
*.idx: No such file or directory (os error 2)
*.lot: No such file or directory (os error 2)
*.fmt: No such file or directory (os error 2)
*.tfm: No such file or directory (os error 2)
...
```

Nguyên nhân là vì `helm-projectile` quá thông minh và nhiệt tình, tới mức nhanh nhẩu, tự add thêm vài cái option kiểu `--ignore *.o ...` vô khi chạy `helm-projectile-ag`, và tất nhiên `rg` không có option này, nên nó hiểu nhầm các giá trị kia là đường dẫn cần search.

Đến đây có hai cách để giải quyết:

1. Là sửa code của `helm-projectile` để nó khỏi thêm vào các option "lạ" nữa
2. Là wrap cái lệnh `rg` trong máy lại thành một lệnh khác để loại bỏ thuộc tính "lạ" kia đi

Cách thứ 1 nếu làm theo thì sẽ ngầu hơn, nhưng khó maintain về sau, nếu không ngại việc tự xài một bản fork riêng cho mọi thứ mình dùng thì cứ chơi.

An toàn nhất là chọn cách 2. Wrap lệnh `rg` lại tức là tạo ra một lệnh mới, đặt ở, ví dụ `/usr/local/bin/rgemacs`, với nội dung như sau:

```bash
#!/usr/bin/env bash
set -euo pipefail

newargs="$(echo "$@" | sed 's/\-\-ignore .* //')"
rg $newargs
```

Đừng quên cấp quyền execute cho nó:

```bash
$ chmod +x /usr/local/bin/rgemacs
```

Rồi thay giá trị của `helm-ag-base-command` thành `rgemacs --vimgrep --no-heading --smart-case`, thế là xong.

**Update 19/11/2018:** Cách trên gây ra một vấn đề đó là khi trong search query có nhiều từ và khoảng trắng, thì chỉ có một từ duy nhất ở cuối query được ghi nhận, ví dụ, search với query: **"day la con ga"** thì chỉ có chữ **"ga"** là được search.

Cách tốt hơn đó là vẫn xài `rg` thay cho `rgemacs` trong `helm-ag-base-command` và ghi đè hàm `helm-projectile-ag` bằng cách chèn đoạn code sau vào Emacs config của bạn, tất nhiên là sau khi load `helm-ag` và `projectile`:

```lisp
(defun helm-projectile-ag (&optional options)
  "Helm version of projectile-ag."
  (interactive (if current-prefix-arg (list (read-string "option: " "" 'helm-ag--extra-options-history))))
  (if (require 'helm-ag nil  'noerror)
      (if (projectile-project-p)
          (let ((helm-ag-command-option options)
                (current-prefix-arg nil))
            (helm-do-ag (projectile-project-root) (car (projectile-parse-dirconfig-file))))
        (error "You're not in a project"))
    (error "helm-ag not available")))
```

Biên lại dựa trên kinh nghiệm và hướng dẫn đọc được từ [một cái gist](https://gist.github.com/pesterhazy/fabd629fbb89a6cd3d3b92246ff29779).
