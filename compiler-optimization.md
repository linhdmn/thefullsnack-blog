# Trình biên dịch tối ưu code như thế nào

Như chúng ta đã biết, khi làm việc với bất kỳ ngôn ngữ nào, quá trình build hoặc biên dịch thường được cung cấp thêm một tùy chọn là **tối ưu hóa code** (code optimization). Trong quá trình này, trình biên dịch (compiler) sẽ tiến hành... tối ưu code của bạn, làm cho nó chạy nhanh hơn, nhẹ hơn.

![](img/code-optimization-cover.jpeg)
<div class="copyright center">Cách mà compiler thực hiện tối ưu hóa</div>

Nhưng cụ thể thì quá trình tối ưu hóa này nó làm gì bên dưới? Để hiểu thêm thì chúng ta hãy cùng đi vào tìm hiểu cơ chế hoạt động và phân tích quá trình tối ưu của trình biên dịch GCC với một chương trình C đơn giản nhé.

## Cơ chế optimize của GCC

GCC thực hiện việc optimize code thông qua rất nhiều các quy tắc, mỗi quy tắc sẽ được biểu diễn thông qua một cờ (optimizatoin flag) mà chúng ta có thể truyền vào cho trình biên dịch.

Chúng ta có thể xem toàn bộ danh sách các optimization flag và cách hoạt động của chúng bằng lệnh:

```
$ gcc --help=optimizers
```

Kết quả sẽ liệt kê ra nhiều lắm, ghi ra đây không hết được, nội dung sẽ có dạng:

```text
  ...
  -funroll-all-loops          Perform loop unrolling for all loops.
  -funroll-loops              Perform loop unrolling when iteration
                              count is known.
  -funsafe-loop-optimizations Allow loop optimizations to assume that
                              the loops behave in normal way.
  -funsafe-math-optimizations Allow math optimizations that may
                              violate IEEE or ISO standards.
  ...
```

Và để cho đơn giản, thì trình biên dịch này quy ước 6 mức độ tối ưu thông qua 6 flag chính đó là **-O0**, **-O1**, **-O2**, **-O3**, **-Os** và **-Ofast**. Khi bạn dùng một trong các flag này, GCC sẽ tự động bật/tắt các optimization flag đã đề cập ở trên tương ứng theo mỗi mức độ.

Để xem chi tiết các loại flag nào được bật/tắt trong từng mức tối ưu, các bạn có thể dùng lệnh:

```
$ gcc -O<level> -Q --help=optimizers
```

Ví dụ với **-O2**, ta có danh sách các flag đã bật/tắt như sau:

```text
  -faggressive-loop-optimizations       [enabled]
  -falign-functions                     [enabled]
  -falign-jumps                         [enabled]
  -falign-labels                        [enabled]
  -falign-loops                         [enabled]
  -fassociative-math                    [disabled]
  -fasynchronous-unwind-tables          [enabled]
  -fauto-inc-dec                        [enabled]
  -fbranch-count-reg                    [enabled]
  -fbranch-probabilities                [disabled]
  -fbranch-target-load-optimize         [disabled]
  ...
```

Bảng sau đây so sánh kết quả giữa các mức độ optimize:

| Mức độ | Mục đích | Chi tiết |
|:------|----------|----------|
| **-O0** | Không tối ưu gì cả | Đây là tùy chọn mặc định. Khi biên dịch bằng flag này, không có quy tắc optimize nào được chạy cả. |
| **-O1** hoặc **-O** | Tối ưu code size và thời gian chạy | Dung lượng đầu ra sẽ nhỏ hơn và thực thi nhanh hơn. Nhưng quá trình biên dịch sẽ chậm hơn. |
| **-O2** | Tối ưu hơn nữa | Tối ưu với nhiều optimization flag hơn và làm cho thời gian thực thi nhanh hơn **-O1**, thời gian biên dịch thì chậm hơn |
| **-O3** | Tối ưu thêm tí nữa | Bật tất cả các optimization flag của **-O2** và thêm vài flag khác, cách này cho thời gian thực thi nhanh nhất nhưng thời gian biên dịch cũng lâu nhất |
| **-Os** | Tối ưu cho kích thước | Bật tất cả những flag không làm tăng code size của **-O2**, kèm theo một vài flag làm giảm code size nữa. |
| **-Ofast** | Tối ưu cho chương trình không cần tính toán chính xác | Bật tất cả các flag của **-O3**, và bật thêm một số flag khác, một số flag làm tăng tốc độ tính toán nhưng giảm mức độ chính xác của các phép toán |

## Một chút về Assembly

Với một chương trình **C**, quá trình biên dịch sẽ trải qua các bước như sau:

<math>
\newcommand{\ra}{\rightarrow}
\newcommand{\tsrc}{\texttt{source files}}
\newcommand{\tass}{\texttt{assembly code}}
\newcommand{\tobj}{\texttt{object code}}
\newcommand{\texe}{\texttt{executable binary}}

\rlap{\overbrace{\phantom{\tsrc\ra\tass}}^\texttt{compiling}}\tsrc\ra
\rlap{\underbrace{\phantom{\tass}\ra\phantom{\tobj}}_\texttt{assembling}}\tass\ra
\overbrace{\tobj\ra\texe}^\texttt{linking}
</math>

Để hiểu rõ hơn về vai trò của từng bước, các bạn có thể tham khảo bài viết [The Four Stages of Compiling a C Program](https://www.calleerlandsson.com/the-four-stages-of-compiling-a-c-program/).

Việc optimize diễn ra trong quá trình **compiling**, tức là quá trình mà GCC biên dịch từ source code thành assembly code. Vì vậy, trong bước phân tích, phần lớn thời gian chúng ta sẽ đọc đống code assembly này.

Nếu các bạn chưa biết gì về Assembly thì cũng không cần phải hoảng loạn =))) mình cũng chỉ biết một vài câu lệnh đơn giản thôi, nên trong quá trình đọc code mình sẽ giải thích nếu gặp các khái niệm lạ.

Nếu các bạn muốn tìm hiểu nhanh về Assembly, có thể đọc tài liệu [x86 Assembly Guide](http://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html) của trường ĐH Yale.

Lý thuyết vậy đủ rồi, giờ bắt tay vào phân tích ví dụ thực té nè.

## Chương trình chuột bạch

Để tiện cho việc phân tích, chúng ta sẽ viết một chương trình không quá phức tạp để làm "chuột bạch".

Tạo một file **main.c** với nội dung là một vòng lặp `for`, như sau:

**main.c**
```c
int main() {
  int a = 0;
  for (int i = 0; i < 50; i++) {
    a += 1;
  }
  return a;
}
```

Chương trình trên tạo ra một biến **a** kiểu số nguyên, với giá trị ban đầu là **0**, sau đó chạy một vòng lặp **50** lần, mỗi lần lặp thì tăng giá trị của **a** lên **1**. Sau đó trả về giá trị của **a**.

## Các mức optimize và Assembly output

Chúng ta sẽ biên dịch chương trình trên với các mức dộ optimize khác nhau, và để xem các optimization flag tối ưu code như thé nào, chúng ta xài thêm flag `-S` để compiler đưa ra file Assembly tương ứng.

**Lưu ý:** Bài viết sử dụng môi trường ArchLinux, với `gcc` phiên bản `6.3.1`, target là `x86_64-pc-linux-gnu`.

### Không tối ưu

Với mức này, bạn có thể biên dịch bằng một trong hai lệnh sau:

```
$ gcc -S main.c -o output.s

# hoặc

$ gcc -O0 -S main.c -o output.s
```

Nội dung của file **output.s** sẽ là:

**output.s**
```
	.file	"main.c"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	$0, -4(%rbp)
	movl	$0, -8(%rbp)
	jmp	.L2
.L3:
	addl	$1, -4(%rbp)
	addl	$1, -8(%rbp)
.L2:
	cmpl	$49, -8(%rbp)
	jle	.L3
	movl	-4(%rbp), %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (GNU) 6.3.1 20170306"
	.section	.note.GNU-stack,"",@progbits
```

Lược bỏ đi một vài thành phần không thực sự  cần thiết cho phạm vi bài viết, chúng ta chỉ cần chú ý đến phần code sau:

```
	.globl	main
main:
.LFB0:
	pushq	%rbp
	movq	%rsp, %rbp
	movl	$0, -4(%rbp)
	movl	$0, -8(%rbp)
	jmp	.L2
.L3:
	addl	$1, -4(%rbp)
	addl	$1, -8(%rbp)
.L2:
	cmpl	$49, -8(%rbp)
	jle	.L3
	movl	-4(%rbp), %eax
	popq	%rbp
	ret
```


### Tối ưu cho code size và execute time

### Tối ưu cao nhất

## Tham khảo thêm

- [1] [Using the GNU Compiler Collection: Optimize Options](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)
- [2] [The Performance Between GCC Optimization Levels](http://www.phoronix.com/scan.php?page=article&item=gcc_47_optimizations&num=1)
- [3] [gcc -o / -O option flags](http://www.rapidtables.com/code/linux/gcc/gcc-o.htm)
- [4] Calle  Erlandsson, [The Four Stages of Compiling a C Program](https://www.calleerlandsson.com/the-four-stages-of-compiling-a-c-program/)
- [5] David Albert, [Understanding C by learning assembly](https://www.recurse.com/blog/7-understanding-c-by-learning-assembly)
- [6] [x86 Assembly Guide](http://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html)
