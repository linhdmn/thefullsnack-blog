---
title: Tự viết Emulator: CHIP-8 Interpreter
published: true
date: 2016-05-01 23:41:04
tags: emulator, javascript, bitwise, assembly, hacking, hardware, game
description: Luôn mơ có một ngày sẽ tự viết được một bộ giả lập cho NES, Gameboy hay thậm chí là Play Station :)) nhưng vẫn dậm chân tại chỗ trong nhiều năm trời chỉ vì không biết phải bắt đầu như thế nào.
image:
---
Nhắc đến game giả lập, chắc không ai lạ gì và ai cũng từng chơi qua (giả lập NES, Gameboy, PS1, PS2,...)

Và chắc hẳn, cũng có không ít bạn nung nấu ý định tự viết một bộ emulator cho riêng mình nhưng không biết bắt đầu từ đâu.

Mình cũng vậy, luôn mơ có một ngày sẽ tự viết được một bộ giả lập cho NES, Gameboy hay thậm chí là Play Station :)) nhưng vẫn dậm chân tại chỗ trong nhiều năm trời chỉ vì không biết phải bắt đầu như thế nào.

Những tài liệu, bài viết nói về giả lập NES hay Gameboy tràn lan trên mạng rất nhiều, nhưng đa số đều bị mình vứt qua một bên vì đọc không hiểu gì cả :sweat_smile: thế nên mình quyết định sẽ tạm gác ý định giả lập máy NES hay Gameboy sang một bên và bắt đầu với một loại đơn giản hơn - đó là **CHIP-8**.

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/pong.png_d4gokk7w44)

Loạt bài này sẽ có 3 phần:

1. **Phần 1:** Giới thiệu CHIP-8 và các khái niệm cơ bản
2. **Phần 2:** Tập lệnh của CHIP-8
3. **Phần 3:** Implement

Các bạn chỉ thực sự đụng đến code từ phần 3, dục tốc bất đạt, vậy nên, cứ bình tĩnh và từ từ mà nuốt trôi hết cái đống khái niệm này. Không thừa đâu :D

----

# Phần 1: Giới thiệu CHIP-8 và các khái niệm cơ bản
---

Vậy CHIP-8 là cái gì? tại sao không phải NES, Gameboy mà lại là CHIP-8?

# Phần 1: Một số khái niệm cơ bản
## Giả lập là gì?
Thực ra chắc không ai thắc mắc cái này đâu, nhưng đề phòng lỡ có ai thắc mắc thì: Giả lập là một phần mềm mô phỏng lại hoạt động của một cái gì đó. Trong trường hợp này, chúng ta đang chuẩn bị viết phần mềm mô phỏng hoạt động của trình thông dịch CHIP-8 (CHIP-8 interpreter)

## CHIP-8 là gì?

CHIP-8 là một ngôn ngữ thông dịch (interpreted programming language), thiết kế bởi Joseph Weisbecker, được dùng cho các máy tính 8-bit COSMAC VIP và Telmac 1800 vào những năm 1970.

![Máy tính Telmac 1800](http://www.chip8.com/telmac1800/telmac1800_1.jpg)

Các máy tính này có đầy đủ các thành phần như màn hình (2 màu đen trắng), âm thanh và bàn phím (nhưng không phải bàn phím QWERTY như bây giờ).

![COSMAC VIP 4 và quả bàn phím khác người của nó](http://www.chip8.com/cosmacvip/CosmacVip4.jpg)

Ngày nay, CHIP-8 vẫn còn được sử dụng trong cộng đồng hobby, với sự đơn giản về cấu trúc cũng như tập lệnh nhỏ (chỉ có 36 lệnh), CHIP-8 Interpreter (thường bị nhầm lẫn là CHIP-8 Emulator) đã trở thành ứng dụng được đa số các lập trình viên lựa chọn khi muốn bắt đầu con đường lập trình emulator của mình.

## Tại sao lại chọn CHIP-8?

Sở dĩ chọn CHIP-8 thay vì NES hay Gameboy là vì cấu trúc đơn giản của nó. Cấu trúc chỉ có một bộ xử lý và số tập lệnh cực kì ít, chưa kể đến khi làm việc với các hệ máy khác, bạn còn phải đau đầu với việc giả lập những module khác, không hề thích hợp cho người mới bắt đầu.

Ngược lại, sau khi hoàn thành được một bản CHIP-8 emulator cho riêng mình, bạn sẽ có đủ kiến thức **cơ bản** để tiếp tục nghiên cứu các hệ máy khác như NES hay Gameboy.

## Chuẩn bị kiến thức
Để bắt đầu con đường lập trình Emulator, bạn chắc chắn cần phải có các kiến thức về:

- Các phép toán thao tác bit (bitwise operation)
- Mã máy (Assembly)

Nếu đọc đến đây mà bạn đã bắt đầu thấy ngại vì không có những kiến thức này thì cũng không sao. Cứ đọc tiếp, trong bài mình sẽ cố gắng giải thích thật kĩ các khái niệm cần thiết. :trollface:

## ROM và nguyên lý hoạt động của CHIP-8
### ROM là gì?
Khi sử dụng bất cứ một phần mềm giả lập cho bất kì hệ mày gì, chúng ta đều nhắc đến khái niệm ROM.

Hiểu đơn giản, ROM là một tập hợp các lệnh thực thi, còn gọi là **opcode** (operation code). Các opcode này là các lệnh được hỗ trợ bởi bộ xử lý.

Ví dụ, đối với CHIP-8, opcode có mã hex là *0x00E0* làm nhiệm vụ xoá toàn bộ màn hình, opcode có mã hex *0xD123* có nhiệm vụ vẽ cái gì đó ra màn hình.

### Nguyên lý hoạt động của CHIP-8
Khi hoạt động, trình thông dịch (intepreter) CHIP-8 sẽ đọc các opcode từ ROM, đưa vào bộ nhớ (RAM) rồi CPU sẽ chạy qua từng opcode đã đọc được và thực thi chúng. Dữ liệu nếu cần sẽ được ghi trở lại vào RAM.

Tóm tắt có thể xem trong hình sau:

![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/fetchexecute.png_zf61jmls9a)

Giả lập mà chúng ta sắp viết, sẽ mô phỏng đúng y quá trình này.

## Các thành phần cơ bản của một thiết bị CHIP-8

Một thiết bị sử dụng CHIP-8 có các thành phần cơ bản sau:

- Bộ nhớ (memory)
- Các thanh ghi (registers)
- Màn hình hiển thị
- Bàn phím, Timers.

Và nhiệm vụ của chúng ta là mô phỏng các thành phần này. Vì thế, chúng ta phải biết cấu trúc, nhiệm vụ, chức năng của mỗi phần.

### Memory

Bộ nhớ cần dùng của một thiết bị CHIP-8 là 4KB (4096 bytes). Được đánh số từ 0x000 (0) đến 0xFFF (4095).

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/memoryMap.png_wonyqqhp6c)

512 bytes đầu tiên (từ 0x000 đến 0x1FF) là vị trí của trình thông dịch (interpreter) và chương trình của chúng ta không thể truy xuất đến các địa chỉ này.

Bắt đầu từ vị trí 0x200 cho đến 0xFFF là nơi mà chương trình của chúng ta được nạp vào.

Và interpreter sẽ bắt đầu chạy chương trình từ vị trí 0x200 (trên các máy ETI 660 thì điểm bắt đầu sẽ là 0x600)

### Registers

![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/REGISTERS.png_4bexqwfunq)

CHIP-8 có 16 thanh ghi đa dụng (general purpose registers) 8 bit, thường được đánh số  là Vx, với x là các số hexa từ 0 đến F.

Thanh ghi VF mặc dù tồn tại nhưng chương trình không thể sử dụng vì nó được dùng làm cờ (flag) cho một số lệnh. Cái này mình sẽ giải thích ở bài sau, khi đi vào chi tiết các lệnh.

Tiếp đến là thanh ghi **I**, được dùng để lưu các địa chỉ trong bộ nhớ. Ở phần sau chúng ta sẽ hiểu rõ cơ chế hoạt động của thanh ghi này hơn.

Ngoài ra còn có 2 timer đặc biệt là Delay Timer và Sound Timer, sẽ nói ở bên dưới. Các register này chứa một giá trị 8 bit.

### Keyboard

Bàn phím của máy tính sử dụng CHIP-8 thường có dạng 16 phím hexa. Khi viết emulator, chúng ta nên map các phím này vào bàn phím máy tính bây giờ. Như hình sau:

![Bên trái là bàn phím hexa, bên phải là bàn phím máy tính thời bây giờ](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/keymapping.png_5tx79z88xo)
_(Bên trái là bàn phím hexa thời cổ đại, bên phải là bàn phím máy tính thời bây giờ)_

Việc map phím như thế nào là tuỳ ý các bạn, nhưng trong bài viết này sẽ map như vậy, nên code cũng sẽ theo layout này.

## Display

Màn hình trên các thiết bị CHIP-8 là màn hình đơn sắc (monochrome), có độ phân giải 64x32 pixel.

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/display.png_ac5l516r70)

Một số máy ETI 660 có độ phân giải 64x48 hoặc 64x64 pixel.

CHIP-8 vẽ các hình ảnh lên màn hình thông qua các sprites. Sprite là một tập hợp nhiều bytes chuyển từ dữ liệu binary tương ứng với hình ảnh mà chúng ta cần vẽ. Mỗi byte có 8 bit, như vậy một sprite sẽ có kích thước 8x**N** (trong đó N là tuỳ ý, từ 1 đến 15)

Các CHIP-8 interpreter thường có một bộ font cài đặt sẵn (built-in), được lưu trong memory (ở vùng 0x000 - 0x1FF) mỗi kí tự trong bộ font này thực chất là các sprite có kích thước 8x5 (tức là có 5 bytes).

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/hexfont.png_kaigppiffk)

## Timers và Sound

Timer, đúng theo tên gọi của nó, thì là một cái bộ đếm (bên điện tử nhiều người gọi là **bộ định thời**). Chức năng của nó đơn giản là: khi được kích hoạt, nó sẽ bắt đầu đếm ngược theo một chu kì thời gian nào đó, đến một lúc nào đó nó sẽ ngừng.

Trong CHIP-8, chúng ta có 2 timers: **Delay timer** và **Sound timer**

**Delay timer** là một bộ đếm được kích hoạt khi giá trị của thanh ghi Delay Timer (DT) - đã đề cập ở phần Register, xem hình - là khác 0. Giá trị này sẽ tự động giảm (trừ 1) liên tục theo chu kì có tần số 60Hz (60 lần 1 giây). Khi nào nó về 0 thì timer sẽ ngưng hoạt động.

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/delayTimer.png_hdj9rkqc88)

Mục đích của Delay timer là làm cho chương trình ngừng lại trong một khoảng thời gian, giống như các hàm `Delay()` hay `Sleep()` ở các ngôn ngữ lập trình khác.

**Sound timer** cũng tương tự như Delay timer, khi giá trị của thanh ghi Sound Timer (ST) khác 0 thì nó sẽ tự động kích hoạt, và giảm giá trị này theo chu kì tần số 60Hz, rồi ngưng khi nó về 0. Nhưng điểm khác biệt là, chừng nào giá trị trong thanh ghi ST khác 0 thì cái loa (buzzer) của máy sẽ hú :))

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/soundTimer.png_uqkre5l3e6)

---

Phù!!! Thế là cũng xong phần lý thuyết. Dài quá nhưng đề nghị các bạn đừng có nhảy cóc. Vì trên đây là những khái niệm rất quan trọng cần phải có trước khi bắt tay vào viết Emulator.

Hy vọng các bạn đã hiểu được cơ chế hoạt động và phần nào hình dung ra được hướng đi mà chúng ta đang thực hiện.

Tiếp theo, mình sẽ giới thiệu và giải thích tập lệnh (instruction set) của CHIP-8, và nói sơ về công dụng của chúng trong thực tế.

# Phần 2: Tập lệnh của CHIP-8
---

# Cấu trúc opcode

Như đã nói, một chương trình CHIP-8 là một tập hợp các `opcode` dưới dạng hexa. Ví dụ, đây là một đoạn nội dung khi đọc file rom game PONG:

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/opcodestruct.png_rvtgccz886)

Mỗi `opcode` có độ dài 2 byte và dược thể hiện bởi 4 kí tự hexa. Chúng ta sẽ đọc 2 byte này và chuyển về mã Assembly đơn giản để dễ hình dung, và công việc sẽ là: implement các chức năng mà các câu lệnh Assembly này thực hiện.

Để lưu vị trí của opcode hiện tại, chúng ta dùng một giá trị gọi là `Program Counter` (PC)

Ví dụ, lệnh đơn giản nhất là `0x00E0`, gồm có 2 byte `00` và `E0`, dịch ra mã máy là `CLS` có nhiệm vụ xoá toàn bộ nội dung màn hình. Hay như hình trên, chúng ta có opcode là `0x82E4`, trong đó có 2 giá trị `x = 2` và `y = E` (sẽ giải thích bên dưới), dịch ra mã máy sẽ là `ADD V2, VE`, có nhiệm vụ cộng giá trị của thanh ghi `VE` vào `V2` (`V2 = V2 + VE`).

### Thứ tự cao thấp của các bit, byte

Mỗi byte có `8 bit`, trong đó theo thứ tự từ phải qua trái, các bit sẽ được gọi tên từ thấp đến cao. Và opcode có 2 byte, như vậy là `16 bit`, và theo thứ tự này, ta có cách gọi tên như hình sau:

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/highlowbyte.png_3adbxv1u1d)

Bit thấp là các bit về phía bên phải, và bit cao là các bit về phía bên trái. Bit cao nhất là bit thứ 1, bit thấp nhất là bit thứ 16.

Byte đầu tiên (1st Byte) được gọi là `Byte cao` (High byte). Byte thứ 2 (2nd byte) được gọi là `Byte thấp` (low byte)

Chúng ta có thể gọi mỗi **4 bit** là một **nibble**.

### Lọc giá trị bằng phép toán AND
Trong các phép toán thao tác bit, phép toán mà chúng ta cần sử dụng nhiều nhất trong quá trình viết Emulator là phép toán `AND`.

Chi tiết về phép toán `AND` bạn có thể xem tại: https://vi.wikipedia.org/wiki/Phép_toán_thao_tác_bit

Nếu dài quá nhác đọc thì phép toán này có thể tóm tắt như hình sau:

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/andop.png_5xt4hz78x5)

Một giá trị khi thực hiện `AND` với `0` thì trả về `0`, và với `F` thì trả về chính nó. Chúng ta dùng đặc tính này để lọc và lấy ra các giá trị cần thiết tại các vị trí mong muốn bên trong một opcode.

Ví dụ:

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/andopsample.png_cmi9maz49x)

# Các tham số trong OPCODE

Ở ví dụ trên, ta thấy opcode `0x82E4` nhận vào 2 tham số `x = 2` và `y = E`, có tất cả 4 loại tham số mà chúng ta cần lấy ra từ opcode, tuỳ theo từng loại/chức năng của opcode.

- **n**: Tham số `n` là **4 bit cuối cùng** (thấp nhất) của toàn bộ opcode, có thể lấy bằng cách sử dụng phép toán `opcode & 0x000F`:

  ```
  // Giả sử: opcode = 0x8C74
  var n = opcode & 0x000F;
  // Kết quả: n = 4
  ```
- **nnn**: Tham số `nnn` là **12 bit thấp nhất** của opcode, là kết quả của phép tính `opcode & 0x0FFF`:

  ```
  // Giả sử: opcode = 0x8C74
  var nnn = opcode & 0x0FFF;
  // Kết quả: nnn = C74
  ```
- **kk**: Tham số `kk` là **8 bit thấp nhất** của opcode, tính bằng: `opcode & 0x00FF`:

  ```
  // Giả sử: opcode = 0x8C74
  var kk = opcode & 0x00FF;
  // Kết quả: kk = 74
  ```
- **x**: Tham số x được xác định bởi **4 bit thấp** nhất của **Byte cao** trong opcode, tức là các bit 5, 6, 7, 8 . Như vậy, ta có thể thực hiện phép toán AND: `opcode & 0x0F00` kết hợp phép toán `dịch bit` (shift) để tìm ra `x`:

  ```
  // Giả sử: opcode = 0x8C74
  var x = (opcode & 0x0F00) >> 8;
  // Kết quả của phép (opcode & 0x0F00) sẽ trả về 0x0C00
  // và ta cần dịch chuyển giá trị trên 8 bit về bên phải (>> 8)
  // để thu được giá trị 0x000C = C, là giá trị ta cần tìm
  // Kết quả: x = C
  ```
- **y**: Tham số `y` được xác định bằng **4 bit cao** của **Byte thấp** trong opcode, tức là các bit 9, 10, 11, 12. Và ta có thể thực hiện phép toán `opcode & 0x00F0` và `shift` để tìm `y`:

  ```
  // Giả sử: opcode = 0x8C74
  var y = (opcode & 0x00F0) >> 4;
  // Kết quả của (opcode & 0x00F0) là 0x0070, nên ta cần
  // dịch chuyển giá trị trên 4 bit về bên trái (>> 4)
  // để thu được giá trị 0x0007 = 7
  // Kết quả: y = 7
  ```

# Tập lệnh của CHIP-8
Bây giờ, chúng ta sẽ cùng tìm hiểu chức năng của từng opcode. Chi tiết cách implement cho từng opcode sẽ có ở bài sau.

Nếu bạn thắc mắc là chúng ta cần biết chức năng của những lệnh này làm gì, thì: bằng cách implement từng lệnh riêng lẽ, chúng ta sẽ tạo được một trình thông dịch mà dựa vào những tập lệnh đó, các lập trình viên có thể kết hợp và viết thành một chương trình hoặc một trò chơi hoàn chỉnh.

Bạn sẽ cần tập lệnh này để tham khảo khi impement.

## Các lệnh về xử lý logic

### 00E0 - CLS
Opcode có giá trị `0x00E0` có thể chuyển thành mã assembly tương ứng là `CLS`, có nhiệm vụ xoá toàn bộ mần hình.

### 1nnn - JP addr
Opcode có dạng `0x1nnn` có mã assembly tương ứng là `JP nnn`, có nhiệm vụ đưa `program counter` đến địa chỉ `nnn` (tức là nhảy đến một đoạn nào đó trong chương trình)

### 2nnn - CALL addr
Opcode có dạng `0x2nnn` có mã assembly tương ứng là `CALL nnn`, có nhiệm vụ gọi một `subroutine` (có thể hiểu là chương trình con) bắt đầu tại vị trí `nnn`. Vị trí hiện tại của `progam counter` trước khi thực hiện việc gọi `subroutine` sẽ được lưu vào `stack`

### 00EE - RET
Opcode có giá trị `0x00EE` có mã assembly tương ứng là `RET`. Khi gặp lệnh này, interpreter sẽ đưa `program counter` về vị trí cuối cùng lưu trong `stack` (tức lf thoát khỏi `sobroutine`/chương trình con)

### 3xkk - SE Vx, byte
Gồm có 2 tham số `x` và `kk`, có nhiệm vụ so sánh giá trị của `Vx` và `kk`, nếu chúng bằng nhau thì bỏ qua (skip) lệnh tiếp theo bằng cách tăng giá trị của `program counter` lên 2.

### 4xkk - SNE Vx, byte
Tương tự như lệnh trên, nếu giá trị của `Vx` khác `kk` thì skip lệnh tiếp theo (tăng `program counter` lên 2)

### 5yx0 - SE Vx, Vy
So sánh giá trị của `Vx` và `Vy`, nếu bằng nhau thì skip lệnh tiếp theo.

### 6xkk - LD Vx, byte
Gán giá trị của `Vx` thành `kk`

### 7xkk - ADD Vx, byte
Đặt giá trị của `Vx` bằng `Vx + kk`

### 8xy0 - LD Vx, Vy
Lưu giá trị của `Vy` vào `Vx`

### 8xy1 - OR Vx, Vy
`Vx` = `Vx` OR `Vy`

Thực hiện phép tính `OR` giữa 2 giá trị `Vx` và `Vy` rồi lưu kết quả vào `Vx`

### 8xy2 - AND Vx, Vy
`Vx` = `Vx` AND `Vy`

Thực hiện phép tính `AND` giữa 2 giá trị `Vx` và `Vy` rồi lưu kết quả vào `Vx`

### 8xy3 - XOR Vx, Vy
`Vx` = `Vx` XOR `Vy`

Thực hiện phép tính `XOR` giữa `Vx` và `Vy` rồi lưu kết quả vào `Vx`

### 8xy4 - ADD Vx, Vy
Gán `Vx` = `Vx` + `Vy`, gán `VF` = `carry` (nhớ)

Giá trị của `Vx` và `Vy` được cộng lại với nhau và lưu vào `Vx`, nếu kết quả lớn hơn `8 bit` (vd: > 255) thì `VF` sẽ được đặt là `1`, ngược lại sẽ là `0`.

### 8xy5 - SUB Vx, Vy
Gán `Vx` = `Vx` - `Vy`, gán `VF` = `NOT borrow` (không mượn)

Nếu `Vx` > `Vy` hiệu số của `Vx - Vy` là không âm, nên `VF` sẽ được gán bằng `1`, ngược lại thì bằng `0`. Kết quả lưu vào `Vx`

### 8xy6 - SHR Vx {, Vy}
Gán `Vx` = `Vx SHR 1`

Nếu bit thấp nhất của `Vx` là 1 thì gán `VF` thàh `1`, ngược lại thì gán bằng `0`.
Gán `Vx` = `Vx / 2`

### 8xy7 - SUBN Vx, Vy
Gán `Vx` = `Vy` - `Vx`, gán `VF` = `NOT borrow` (không mượn)

Nếu `Vy` > `Vx` thì gán `VF` thành `1`, ngược lại gán thành `0`. Hiệu số lưu vào `Vx`.

### 8xyE - SHL Vx {, Vy}
Gán `Vx` = `Vx SHL 1`

Nếu bit cao nhất của `Vx` là `1` thì gán `VF` thành `1`, ngược lại, gán thành `0`. Cuối cùng gán `Vx = Vx * 2`

### 9xy0 - SNE Vx, Vy
Skip lệnh tiếp theo nếu `Vx` != `Vy`

### Annn - LD I, addr
Lưu giá trị `nnn` vào thanh ghi `I`

### Bnnn - JP V0, addr
Đưa `program counter` tới vị trí `nnn + V0`

### Cxkk - RND Vx, byte
Gán giá trị `Vx` = `random byte` AND `kk`

Interpreter sẽ khởi tạo một số ngẫu nhiên (random) có giá trị từ `0` đến `255`, sau đó `AND` với giá trị của `kk`. Kết quả lưu vào `Vx`

## Các lệnh tương tác (display, keyboard, sound...)

### Dxyn - DRW Vx, Vy, nibble
**Vẽ ra màn hình - Đây là lệnh quan trọng nhất trong số tất cả các lệnh**

Interpreter sẽ đọc `n` byte từ bộ nhớ, bắt đầu từ địa chỉ được lưu trong thanh ghi `I`. Các byte này sẽ được hiển thị dưới dạng một `sprite` trên màn hình từ toạ độ (`Vx`, `Vy`).

Sprite được vẽ ra màn hình theo phép `XOR`, nếu có pixel nào bị xoá vì phép toán này thì `VF` sẽ được gán là `1`, ngược lại thì gán bằng `0`.

Nếu các điểm của sprite nằm ở bên ngoại phạm vi hiển thị của màn hình thì sẽ được vẽ ra ngay tại các cạnh biên của màn hình gần với nó nhất.

### Ex9E - SKP Vx
Skip lệnh tiếp theo nếu phím có giá trị của `Vx` được nhấn.

Kiểm tra bàn phím, nếu phím được nhấn có giá trị (key code) bằng vơi giá trị của `Vx` thì `program counter` sẽ được tăng lên `2`.

### ExA1 - SKNP Vx
Skip lệnh tiếp theo nếu phím có giá trị của `Vx` không được nhấn.

Tương tự như lệnh trên, nhưng lệnh này tăng `PC` lên `2` nếu phím có key code là `Vx` không được nhấn xuống.

### Fx0A - LD Vx, K
Chờ bắt sự kiện nhấn phím, lưu `key code` vào `Vx`

Lệnh này sẽ ngừng chương trình cho tới khi có phím được nhấn.

### Fx07 - LD Vx, DT
Gán `Vx` = `delay timer value`

Lưu giá trị của thanh ghi `DT` vào thanh ghi `Vx`

### Fx15 - LD DT, Vx
Gán `delay timer` = `Vx`

Gán giá trị của thanh ghi `DT` là `Vx` để bắt đầu thực hiện việc chờ (delay), xem giải thích về `Delay Timer` ở bài trước.

### Fx18 - LD ST, Vx
Gán `sound timer` = `Vx`

Gán giá trị của thanh ghi `ST` thành `Vx` để bắt đầu thực hiện phát âm thanh, xem giải thích về `Sound Timer` ở bài trước.

### Fx1E - ADD I, Vx
Gán `I` = `I` + `Vx`

Lưu tổng số của `I` và `Vx` vào `Vx`

### Fx29 - LD F, Vx
Gán `I` = `vị trí của sprite kí tự Vx`

Gán giá trị của thanh ghi `I` thành vị trí của kí tự hex font dựng sẵn tương ứng với `Vx`.

### Fx33 - LD B, Vx
Interpreter lấy giá trị thập phân của `Vx`, lưu các số hàng trăm vào bộ nhớ ở vị trí `I`, các số hàng chục vào vị trí `I + 1`, các số hàng đơn vị ở vị trí `I + 2`

### Fx55 - LD [I], Vx
Lưu giá trị từ thanh ghi `V0` vào các thanh ghi `Vx` trong bộ nhớ, bắt đầu từ địa chỉ trong `I`. Sang phần sau sẽ rõ hơn trong quá trình implement.

### Fx65 - LD Vx, [I]
Đọc giá trị từ các thanh ghi `Vx`, bắt đầu từ `I` vào `V0`

---

Trên đây là toàn bộ các opcode mà chúng ta sẽ implement ở phần sau. Sau khi implement tất cả các opcode này thì chúng ta có thể load một ROM game mẫu và chơi thử.

Có một số lệnh của Super CHIP-8 nhưng để bài viết đơn giản, mình sẽ không đề cập đến. Sau này nếu có thời gian thì chúng ta sẽ implement thêm sau.

# Phần 3: Implement
---

Chi tiết về cách implement các bạn có thể tham khảo mã nguồn tại: https://github.com/huytd/js-chip8-emulator
