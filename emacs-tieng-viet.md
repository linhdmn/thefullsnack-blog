---
title: Gõ tiếng Việt trên Emacs Linux
published: true
date: 2018-05-04 16:30:30
tags: emacs, linux
description: 
image:
---
Có một vấn đề khá đau đầu với dân xài Emacs trên Linux (dù là GUI hay Termminal mode), đó là không gõ tiếng Việt với `fcitx` hay `ibus` được.

Có một cách khắc phục, nhưng cách này chỉ hoạt động nếu bạn dùng Emacs trên terminal, còn GUI mode thì vẫn bó tay.

Đầu tiên, cần phải kiẻm tra xem máy đã enabled `vi_VN.UTF-8` locale chưa:

```
locale -a
```

Nếu chưa thấy, thì có thể add thêm thông qua hướng dẫn tại [Archlinux Wiki/Locale](https://wiki.archlinux.org/index.php/locale#Generating_locales).

Tiếp theo, cài đặt biến môi trường `LC_CTYPE` của máy thành `vi_VN.UTF-8`:

```
export LC_CTYPE=vi_VN.UTF-8
```

Từ bây giờ, bạn đã có thể gõ tiếng Việt trên Emacs (chỉ trong terminal mode `emacs -nw` thôi nhé**.

---

**Update Ngày 20/5/2018**

Tình hình là mình vừa update lên Emacs 27, nên quyết định ngồi config lại lần nữa, quyết tâm gõ cho bằng được tiếng Việt trên Emacs =))) thế mà lại gõ được thiệt luôn :think-hopeful: không biết từ bản 26, 27 có update gì không, nhưng khả năng là do các config mới và cài thêm gói `ibus-el`. Cách làm như sau:

Đầu tiên, update Emacs, hoặc cài bản mới, xóa bản cũ:

```
yaourt -R emacs
yaourt -S emacs-git
```

Tiếp theo, cài đặt các biến môi trường để Ibus hoạt động trên các môi trường như GTK, QT (Emacs mặc định xài GTK):

```
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
export CLUTTER_IM_MODULE=ibus
export LC_CTYPE=vi_VN.UTF-8
```

Tiếp, cài `ibus-el`, có thể tải về từ [launchpad.net](https://launchpad.net/ibus.el) hoặc [github](https://github.com/pkg-ime/ibus-el), giải nén vô `.emacs.d` và load nó lên, tùy theo cách bạn quản lý package như nào.

Mình xài Spacemacs, nên mình giải nén nó vô thư mục `~/.emacs.d/private/local/ibus-el`, và cấu hình cho Emacs load như sau:

```lisp
dotspacemacs-additional-packages '(
  ...
  (ibus-el :location local)
)
```

Load trong `user-config`:

```lisp
(defun dotspacemacs/user-config ()
  ...
  ;; Ibus
  (require 'ibus)
  (add-hook 'after-init-hook 'ibus-mode-on)
  (setq ibus-cursor-color '("red" "blue" "limegreen"))

  ...
)
```

Nếu máy bạn mặc định xài Python 3, thì phải cấu hình thêm biến `ibus-python-shell-command-name` về Python 2:

```
M-x custom-set-variable

Chọn: ibus-python-shell-command-name

Gõ: /bin/python2
```

Hoặc:

```lisp
(custom-set-variables
 '(ibus-python-shell-command-name "/bin/python2")
)
```

Thế là xong.
