---
title: OrgMode trên Vim
published: true
date: 2017-02-02 14:45:40
tags: vim, emacs
description: 
image:
---
**OrgMode** là một chức năng rất hữu dụng trên Emacs.

Bạn có thể sử dụng **OrgMode** để quản lý công việc (todo list), hay làm công cụ soạn thảo nội dung có trình bày một cách hệ thống,... tùy theo nhu cầu cá nhân.

Trên Vim, chúng ta cũng có thể sử dụng **OrgMode** bằng cách cài plugin `vim-orgmode`.

## Cài đặt 

Cài với VimPlug hoặc Vundle,... như sau:

```
Plug 'jceb/vim-orgmode'
Plug 'tpope/vim-speeddating'
```

Ngoài plugin này chúng ta có cài thêm plugin `vim-speeddating` để sử dụng chức năng insert ngày tháng.

Chúng ta có thể cấu hình một vài phím tắt thông dụng (sẽ giải thích bên dưới):

```
" OrgMode Keys
nnoremap <Leader><Tab> :OrgCheckBoxToggle<CR>
nnoremap <Leader>nc :OrgCheckBoxNewBelow<CR>
nnoremap <Leader>nd :OrgDateInsertTimestampActiveCmdLine<CR>
nnoremap <Leader>td :OrgBufferAgendaTodo<CR>
nnoremap <Leader>wa :OrgBufferAgendaWeek<CR>
```

## Sử dụng

Sau khi cài đặt xong thì Vim sẽ tự động chuyển sang `orgmode` khi mở một file `*.org` bất kì.

Giả sử chúng ta có file `~/today.org`, mở file này với vim bạn sẽ thấy kí hiệu `org` ở phần filetype, cho biết đây là file mở dạng `orgmode`, nếu không thấy thì chúng ta có thể set filetype tự động bằng lệnh:

```
:set ft=org
```

### Heading

Heading là các đầu mục để bạn có thể dễ dàng phân loại, tổ chức nội dung của mình (tương tự các thẻ `h1`, `h2`,... trong HTML)

#### Chèn heading

Để chèn một đầu mục (heading), bạn dùng các dấu sao `*` theo thứ tự của nó, ví dụ:

```
* Heading level 1
** Heading level 2
*** Heading level 3
** Another Heading level 2
```

#### Di chuyển giữa các heading

Khi đứng tại một heading, bạn có thể gõ `}` hoặc `{` để nhảy nhanh tới các heading tiếp theo/trước đó.

#### Nâng cấp, giáng cấp heading

Bạn cũng có thể chuyển một heading level 1 thành level 2 (giáng cấp - demote) hoặc một heading level 3 thành level 2 (nâng cấp - promote) bằng cách gõ `>>` hoặc `<<`.

#### Đóng/mở heading

Đứng tại một heading, bạn có thể nhấn phím `TAB` hoặc `SHIFT-TAB` để đóng (collapse) hoặc mở (expand) heading đó và tất cả các heading con bên trong nó theo thứ tự xoay vòng:

```
  ,-> FOLDED -> CHILDREN -> SUBTREE --.
  '-----------------------------------'
```

### Task

Trong mỗi heading, bạn có thể gõ vào bất kì nội dung gì bạn muốn. Trong trường hợp sử dụng `orgmode` để làm todo list, thì nội dung bạn gõ nhiều nhất đó chính là các task.

#### Chèn task 

Để chèn một task ta có thể chèn theo cú pháp sau:

```
- [ ] <nội dung task>
```

Đây là một task dạng check box để chúng ta có thể tick vào hoặc bỏ tick nó bất cứ lúc nào.

Để chèn nhanh, có thể xài phím tắt đã config ở trên kia là `<Leader>nc` (**n-c** có nghĩa là new check).

#### Tick và bỏ tick

Để tick vào một task, bạn điền dấu `X` vào phần trống giữa dấu ngoặc vuông `[ ]`:

```
- [X] <nội dung task>
```

Hoặc tick tự động bằng phím `<Leader><Tab>`.

Lặp lại thao tác này sẽ bỏ tick.

### Làm việc với agenda

Agenda là một chức năng cực kì hay giúp tổng hợp nội dung trên file `org` hiện tại thành một bảng tóm tắt, cho chúng ta cái nhìn tổng quan về các task ở trạng thái `TODO`, hoặc tóm tắt theo ngày, theo tuần.

#### Trạng thái TODO/DONE

Bạn có thể thêm trạng thái `TODO` hoặc `DONE` vào đầu mỗi heading để dễ dàng theo dõi và để khởi tạo agenda, ví dụ:

```
* Today
** TODO Project A
  ...
** DONE Project B
```

Nội dung như trên có nghĩa là mục `Project A` đang ở trạng thái `TODO` và mục `Project B` ở trạng thái `DONE`.

Các nội dung có trạng thái `DONE` sẽ không được hiện ra khi xem agenda.

#### Chèn ngày tháng 

Để chèn ngày tháng tại vị trí hiện tại của con trỏ, chúng ta dùng lệnh `<Leader>nd` (**n-d** là new date).

Dòng lệnh hiện ra cho phép bạn chọn ngày hiện tại bằng cách nhấn `<Enter>`, hoặc tăng giảm số ngày bằng cách gõ vào, ví dụ: `+1d` (tăng 1 ngày), `+4w` (tăng 4 tuần), `-5m` (giảm 5 tháng),...

#### Xem agenda các task TODO

Để xem danh sách các mục có trạng thái `TODO`, ta gõ `<Leader>td` (**t-d** tức là todo).

Khi ở màn hình danh sách To Do, bạn có thể dùng phím `<Tab>` để nhảy đến nội dung tương ứng trên file.

#### Xem agenda trong tuần 

Để xem danh sách các task cần làm trong tuần, chúng ta dùng lệnh `<Leader>wa` (**w-a** là weekly agenda).

---

Trên đây là một vài thao tác cơ bản để bạn có thể bắt đầu sử dụng `vim-orgmode` một cách hiệu quả. Trong quá trình sử dụng, bạn có thể tìm hiểu thêm thông qua hướng dẫn sử dụng của `vim-orgmode` [trên Github](https://github.com/jceb/vim-orgmode/blob/master/doc/orgguide.txt) để làm việc hiệu quả hơn nhé.

Happy vimming ^^
