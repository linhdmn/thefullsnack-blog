---
title: Cài đặt Rust trên Arch Linux
published: true
date: 2017-04-28 20:17:50
tags: rust, linux, vim
description: 
image:
---
Việc cài đặt Rust trên môi trường Arch Linux khá là đơn giản. **pacman** có sẵn gói [`rust`](https://www.archlinux.org/packages/?name=rust) và [`cargo`](https://www.archlinux.org/packages/?name=cargo), bạn có thể chọn cách cài đặt trực tiếp 2 packages này.

Tuy nhiên ở bước cài đặt công cụ hỗ trợ cho các IDE, chúng ta thường dùng [`racer`]() và engine này yêu cầu chúng ta phải có source code của Rust nằm sẵn trong máy. Nếu chọn cách cài đặt từng gói, thì chúng ta có thể cài đặt Rust source code thông qua **yaourt** với gói [`rust-src`](https://aur.archlinux.org/packages/rust-src/)<sup>`AUR`</sup>, rồi phải set biến môi trường `RUST_SRC_PATH` khá là phiền phức.

Cho nên, cách cài đặt đơn giản nhất đó là dùng [`rustup`](https://www.archlinux.org/packages/?name=rustup). Package này gồm có: `rustup`, `rustc` và `cargo`. Và bước cài đặt Rust source cũng đơn giản hơn.

Các bước cài đặt cụ thể như sau:

## Cài Rustup

Cài `rustup` thông qua `pacman` sau đó chọn phiên bản Rust cần dùng, ví dụ ở đây dùng bản stable.

```
$ sudo pacman -S rustup
$ rustup default stable
```

Sau khi cài xong thì chúng ta có toàn bộ các công cụ cần thiết để làm việc với rust. Có thể kiểm tra thông qua các lệnh:

```
$ rustc -V
$ cargo -V
```

## Cài đặt công cụ hỗ trợ cho các IDE

Việc đầu tiên cần làm trước khi cài đặt các gói hỗ trợ cho IDE là add Rust source code vào máy, thực hiện việc này thông qua `rustup` như sau:

```
$ rustup component add rust-src
```

Sau khi đã có source code của Rust trong máy, các bạn có thể bắt đầu đọc source để hiểu những gì Rust làm bên dưới, từ đó sẽ nắm được toàn bộ kiến thức và không cần dùng bất cứ trình hỗ trợ nào cho IDE khi code Rust nữa... à nhầm, không phải =)))) bước tiếp theo là cài đặt [`racer`](https://github.com/phildawes/racer).

```
$ cargo install racer
```

Cuối cùng, tùy theo IDE/editor mà bạn sử dụng, bạn có thể cài các plugin khác nhau cho việc support Rust. Nếu bạn xài Vim thì cài plugin [`rust.vim`](https://github.com/rust-lang/rust.vim):

```
Plug 'rust-lang/rust.vim'
```

## Cài đặt Syntastic trong Vim

Phần này chỉ là phụ, dành cho bạn nào xài Vim và muốn có chức năng kiểm tra lỗi trực tiếp trong lúc code bằng `Syntastic`.

Cài thêm plugin `Syntastic` như sau:

```
Plug 'vim-syntastic/syntastic'
```

Cấu hình cho `Syntastic` trong `.vimrc` theo như recommend trên trang chủ dự án của họ:

```
set statusline+=\ %#warningmsg#
set statusline+=\ %{SyntasticStatuslineFlag()}
set statusline+=\ %*

let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0

```

Tuy nhiên nếu chỉ đến đây, khả năng là chức năng kiểm tra lỗi vẫn chưa hoạt động được, cần thêm vào dòng sau để chỉ định Syntastic checker dành cho Rust:

```
let g:syntastic_rust_checkers = ['rustc']
```

Khởi động lại Vim và thưởng thức thành quả :D

Đến đây thì việc cài đặt hoàn tất. Bạn có thể tham khảo thêm [wiki của Arch Linux](https://wiki.archlinux.org/index.php/Rust) để biết thêm chi tiết về cách cài đặt Rust và các chức năng cần thiết khác.

**UPDATE:** Sau khi làm việc với Rust theo như setup ở trên vài ngày thì mình có gặp hai vấn đề liên quan đến `cargo`, đó là:

- `Syntastic` không thể nhận biết được project dạng **library**, nên sẽ luôn xuất hiện thông báo kiểu như `missing main function`
- Khi sử dụng các `crate` bên ngoài thì `Syntastic` sẽ báo lỗi ngay dòng `extern crate ...`, nói là không tìm thấy crate này

Hai vấn đề trên buộc lòng chúng ta phải sử dụng `cargo` làm checker cho `Syntastic` thay vì `rustc`, hiện tại thì `cargo check` đã được merge vào master của `cargo` nhưng việc support này vẫn chưa được merge vào plugin `rust.vim` (xem pull request [rust.vim#132](https://github.com/rust-lang/rust.vim/pull/132)).

Cách giải quyết tạm thời là cài `rust.vim` trực tiếp từ repo của tác giả **jlevesy** thay vì từ repo của **rust-lang**:

```
Plug 'jlevesy/rust.vim'
```

Và chỉ định `cargo` làm checker:

```
let g:syntastic_rust_checkers = ['cargo']
```
