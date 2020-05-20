---
title: TDD qua ví dụ thực tế
published: true
date: 2016-07-24 13:37:29
tags: tdd, ruby, fizzbuzz
description: Một trong những câu hỏi được đặt ra nhiều nhất khi nghiên cứu về TDD có lẽ là: **Áp dụng TDD như thế nào trong thực tế?** 
image:
---
TDD (Test Driven Development) - tức là một phương pháp lập trình chú trọng vào việc test, "viết test trước viết code sau",... rất nhiều người đã thử tìm hiểu về TDD và đều đọc được những định nghĩa như thế này trong các bài viết, nhưng cuối cùng khi đọc xong thì vẫn không hiểu nổi TDD là gì.

Một trong những câu hỏi được đặt ra nhiều nhất khi nghiên cứu về TDD có lẽ là: **Áp dụng TDD như thế nào trong thực tế?** 

![](http://www.seleniumframework.com/wp-content/uploads/2014/10/TestDrivenDevelopment_155FE5992.png)

Và hình như cũng không có nhiều bài viết giải đáp được câu hỏi này. Kết quả là rất nhiều người không hiểu được nó nên không áp dụng được hoặc áp dụng sai cách, từ đó tạo ra những hiểu lầm hay "ảo tưởng sức mạnh" về nó, rất là tai hại.

Vậy thế nào là TDD? Và thế nào mới là hiểu đúng về TDD? Mình xin không trả lời trực tiếp, mà sẽ trình bày một ví dụ minh họa áp dụng TDD để qua đó các bạn có thể tự rút ra được câu trả lời cũng như quyết định xem có nên áp dụng nó hay không :D Còn bây giờ thì chúng ta bắt đầu thôi.

## Bài toán FizzBuzz

Để minh họa việc áp dụng TDD trong thực tế, mình sẽ áp dụng nó vào việc xây dựng một chương trình đơn giản có tên là **FizzBuzz**.

Nếu các bạn chưa biết, thì FizzBuzz là một hàm nhận vào một số **N** và trả về các giá trị khác nhau tùy theo từng điều kiện:

- Trả về chuỗi **"Fizz"** nếu số **N** chia hết cho **3** (ví dụ 3, 6, 9,...)
- Trả về chuỗi **"Buzz"** nếu số **N** chia hết cho **5** (ví dụ 5, 10,...)
- Trả về chuỗi **"FizzBuzz"** nếu số **N** chia hết cho cả **3** và **5** (ví dụ 15, 45,...)
- Trả về chính con số đó nếu nó không thỏa mãn các điều kiện trên

Sỡ dĩ mình chọn bài toán này để minh họa cho TDD là vì nó cực kì đơn giản mà lại có nhiều case tùy thuộc vào từng input, không quá khó để hiểu và cover được toàn bộ các case trong phạm vi 1 bài viết ngắn như thế này.

## Implement bài toán FizzBuzz áp dụng TDD

Rồi, giờ thì chúng ta đã hiểu đề bài, vậy thì bắt tay vào code thôi. Trong bài này mình sẽ sử dụng **Ruby** và **Minitest** để implement và test. Nếu lần đầu tiên nghe nói đến 2 cái này thì các bạn chịu khó tìm đọc qua một chút trước khi chúng ta tiếp tục. Nhất là Minitest, còn Ruby thì code của nó cũng không đến nỗi quá phức tạp nên dù bạn code Java hay JavaScript thì đọc vào chắc cũng sẽ hiểu :trollface:

### Chuẩn bị hành lý

Rõ ràng khi bắt đầu code thì phải setup những thứ như là cấu trúc thư mục blah blah. Nhưng phần này không liên quan đến TDD, nên mình sẽ nói lướt qua. Nếu các bạn không hiểu thì có thể hỏi trong phần comment nhé. 

Tạo một thư mục dành cho project của chúng ta, có thể đặt tên tùy ý, trong đó tạo 3 file lần lượt như sau: **Rakefile**, **FizzBuzz.rb** và **FizzBuzz_test.rb**

**Rakefile** có nhiệm vụ định nghĩa task chạy test, có nội dung như sau:

```
require "rake/testtask"

Rake::TestTak.new(:test) do |t|
  t.libs << "test"
  t.test_files = FileList["*_test.rb"]
  t.verbose = true
end

task :default => :test
```

Chúng ta sẽ implement module **FizzBuzz**, trong module này có 1 hàm **run()** nhận vào tham số N và trả về các giá trị như đã nói ở phần khái niệm. Đây là phần sườn code cho module **FizzBuzz**, nằm trong file **FizzBuzz.rb** 

```
module FizzBuzz
  extend self
	
  def run(n)
	
  end
end
```

Phần code test sẽ đặt trong file **FizzBuzz_test.rb** và có nội dung như sau:

```
require "minitest/autorun"
require "./FizzBuzz"

class FizzBuzzTest < Minitest::Test
	
end
```

Sau khi tạo xong 3 file trên, các bạn có thể chạy thử test bằng lệnh:

```
rake test

# hoặc là

rake
```

Output sẽ như thế này:

```
# Running:



Finished in 0.001125s, 0.0000 runs/s, 0.0000 assertions/s.

0 runs, 0 assertions, 0 failures, 0 errors, 0 skips
```

Output như trên nghĩa là bạn đã cài đặt thành công chương trình, nhưng chưa có test case nào được chạy.

Giờ chúng ta sẽ bắt tay vào phần chính, test và code.

### Chờ chút!

À, mà trước khi bắt đầu thì cũng nên nhắc lại một tí về TDD. TDD tức là "viết test trước khi viết code". Nghĩa là sao? Chưa có code thì làm sao mà test? 

Đây chính là mấu chốt, khi bạn định implement một function nào đó, bạn sẽ phải viết một function khác, sử dụng chính cái function bạn định implement, và khi chạy test tất nhiên nó sẽ fail, đơn giản là vì bạn chưa implement cái gì cả. Vì vậy việc tiếp theo là implement để làm cho nó hết fail (pass). Cuối cùng, bỏ chút thời gian ra refactor lại code cho đẹp và gọn gàng hơn, để lỡ có bị ai đọc vào thì họ cũng khôi có chửi được mình =))))

![](http://luizricardo.org/wordpress/wp-content/upload-files/2014/05/tdd_flow.gif)

Và bằng cách suy nghĩ trước mọi tình huống có thể xảy ra khi sử dụng cái function bạn sắp viết, sau khi toàn bộ test đã pass thì bạn có một function hoàn hảo và không còn khả năng xảy ra bug nữa. 

Quay trở lại bài toán, chúng ta sẽ có bao nhiêu tình huống cần nghĩ ra để test? Bạn thử tự trả lời trước khi đọc tiếp xem?

Câu trả lời là **4**. Cũng chính là 4 gạch đầu dòng mô tả hàm **FizzBuzz** đã ghi ở trên. Tất nhiên nếu máu thì bạn có thể viết nhiều test case hơn cho nó cũng được, nhưng nếu bạn viết quá nhiều test case thì rất có thể bạn đang viết thừa rồi (tức là sẽ có các test case test cùng một vấn đề).

Các bạn thấy sao? Tui nói nhiều quá hả? À không, TDD lợi hại quá hả? Thôi bắt đầu nhé, giờ chúng ta test cái case đầu tiên nhé.

### Test case #1: Nhận vào một số chia hết cho 3, trả về chuỗi Fizz

Xem nào, như đã nói ở phần trên kia thì chúng ta sẽ gọi hàm `FizzBuzz.run(...)`, với tham số **n** truyền vào cho hàm `run` là một con số chia hết cho 3, ví dụ như là số 6, hoặc 9. Và kết quả thu được sẽ là một chuỗi **"Fizz"**.

#### Viết test trước...

Vậy thì test case đầu tiên của chúng ta sẽ như thế này:

**FizzBuzz_test.rb** 

```
def test_fizzbuzz_run_return_fizz
  expect = "Fizz"
  actual = FizzBuzz.run(6)
  assert_equal expect, actual
end
```

Đặt tên hàm có dạng `test_xxx` là do Minitest quy định, và để cho rõ ràng thì phần `xxx` trong tên hàm test chúng ta nên diễn đạt nội dung hoặc mục đích của test case này, ở đây là: _FizzBuzz Run Return Fizz_, có nghĩa là: _Hàm `run` của module `FizzBuzz` sẽ trả về nội dung Fizz_.

Việc khai báo 2 biến `expect` và `actual` là để cho phần code test này dễ đọc hơn thôi, trong thực tế các bạn có thể bỏ nếu không thích. Tuy nhiên tiêu chí là: Code test thì phải luôn luôn rõ ràng.

Lệnh `assert_equal` cũng là của Minitest, có nhiệm vụ kiểm tra xem giá trị của 2 tham số `expect` và `actual` có bằng nhau không. Ngoài ra còn có nhiều phép assertion khác, các bạn có thể đọc thêm [tại đây](http://docs.seattlerb.org/minitest/Minitest/Assertions.html)

Giờ chạy test thử nhé, bảo đảm nó bắn lỗi ngay:

```
# Running:

F

Finished in 0.001543s, 2592.3526 runs/s, 2592.3526 assertions/s.

  1) Failure:
FizzBuzzTest#test_fizzbuzz_run_return_fizz [...]:
Expected: "Fizz"
  Actual: nil
  
1 runs, 1 assertions, 1 failures, 0 errors, 0 skips
rake aborted!
```

Nội dung thông báo trên có nghĩa là, kết quả mong đợi là **"Fizz"** nhưng kết quả thực tế lại là **nil** (null, vì chưa implement lấy đâu ra có kết quả).

#### ...Viết code sau

Test cũng viết rồi, fail cũng fail luôn rồi, giờ là lúc làm cho cái test kia xanh trở lại (pass), bằng cách implement trường hợp đầu tiên cho hàm `run` như đoạn code dưới đây:

**FizzBuzz.rb**

```
def run(n)
  if n % 3 == 0
    return "Fizz" 
  end
end
```

Quá đơn giản! Giờ chạy test lại thử xem nhé:

```
# Running:

.

Finished in 0.001766s, 566.2514 runs/s, 566.2514 assertions/s.

1 runs, 1 assertions, 0 failures, 0 errors, 0 skips

```

Ngon! Ghi vậy tức là pass rồi đó. Giờ đến phần refactor, cơ mà vì cái hàm này cũng đơn giản quá nên thôi mình bỏ qua bước này vậy.

Giờ qua test case thứ 2 nhé.

### Test case #2: Nhận vào một số chia hết cho 5, trả về chuỗi Buzz

Trường hợp thứ 2 là hàm `run()` nhận vào một số **n** chia hết cho 5 (nhưng nhớ là không chia hết cho 3 nhé), ví dụ 5, 10, 20,... và nó sẽ phải trả về kết quả là chuỗi **"Buzz"**. 

#### Viết test trước...

Vậy thì hàm test thứ 2 sẽ như sau:

**FizzBuzz_test.rb**

```
def test_fizzbuzz_run_return_buzz
  expect = "Buzz"
  actual = FizzBuzz.run(10)
  assert_equal expect, actual
end
```

Chạy thử luôn cho nó fail:

```
# Running:

F.

Finished in 0.001374s, 1455.6041 runs/s, 1455.6041 assertions/s.

  1) Failure:
FizzBuzzTest#test_fizzbuzz_run_return_buzz [/Users/huy/Desktop/Code/rbfizzbuzz/FizzBuzz_test.rb:14]:
Expected: "Buzz"
  Actual: nil

2 runs, 2 assertions, 1 failures, 0 errors, 0 skips
rake aborted!
```

Output như trên có nghĩa là chạy 2 test case mà có 1 cái fail. Kết quả mong đợi của cái test fail là **"Buzz"** còn output thì lại là **nil**. Vì chúng ta chưa implement trường hợp số chia hết cho 5 mà ko chia hết cho 3, nên nó ko có return cái gì về cả.

#### ...Viết code sau

Chúng ta implement trường hợp này như sau:

**FizzBuzz.rb**

```
def run(n)
  if n % 3 == 0
    return "Fizz"
  elsif n % 5 == 0
    return "Buzz"
  end
end
```

Giờ thì chạy thử coi nó pass chưa nào:

```
# Running:

..

Finished in 0.001176s, 1700.6803 runs/s, 1700.6803 assertions/s.

2 runs, 2 assertions, 0 failures, 0 errors, 0 skips
```

Ngon lành! Đi tiếp nào.

### Test case #3: Nhận vào một số cùng chia hết cho 3 và 5, trả về chuỗi FizzBuzz

Trường hợp thứ 3 là chạy hàm `run()` với input là một số **n** cùng chia hết cho **3** và **5**, ví dụ như là 15, 30, 45,...

#### Viết test trước...

Vẫn như các bước trên, có vẻ hơi chán rồi nhỉ :laughing: 

**FizzBuzz_test.rb**

```
def test_fizzbuzz_run_return_fizzbuzz
  expect = "FizzBuzz"
  actual = FizzBuzz.run(15)
  assert_equal expect, actual
end
```

Chạy cho nó fail nào... :sleeping: 

```
# Running:

.F.

Finished in 0.001382s, 2170.7670 runs/s, 2170.7670 assertions/s.

  1) Failure:
FizzBuzzTest#test_fizzbuzz_run_return_fizzbuzz [...]:
Expected: "FizzBuzz"
  Actual: "Fizz"

3 runs, 3 assertions, 1 failures, 0 errors, 0 skips
rake aborted!
```

OK lỗi rồi. Giờ implement nào, mà khoan, sao lần này giá trị actual không phải **nil** mà là **Fizz** nhỉ. Thôi kệ. Bỏ qua.

#### ...Viết code sau

Vậy là giờ chúng ta phải implement cho trường hợp cùng chia hết cho cả **3** và **5**. Làm như thế nào nhỉ?

**FizzBuzz.rb**

```
def run(n)
  if n % 3 == 0
    return "Fizz"
  elsif n % 5 == 0
    return "Buzz"
  elsif n % 3 == 0 && n % 5 == 0
    return "FizzBuzz"
  end
end
```

Ngon lành chưa? Hmm, hình như có gì đó ko đúng lắm, thôi kệ, chắc lo xa quá thôi, test nào.

```
# Running:

.F.

Finished in 0.001588s, 1889.1688 runs/s, 1889.1688 assertions/s.

  1) Failure:
FizzBuzzTest#test_fizzbuzz_run_return_fizzbuzz [/Users/huy/Desktop/Code/rbfizzbuzz/FizzBuzz_test.rb:20]:
Expected: "FizzBuzz"
  Actual: "Fizz"

3 runs, 3 assertions, 1 failures, 0 errors, 0 skips
rake aborted!
```

Ớ, vẫn lỗi. Sao kì zẩy, rõ ràng là fail nè. Implement rồi mà fail là sao ta. Xem nào, nó fail vì actual trả về **"Fizz"**, kì lạ... Có gì đó sai rồi, cùng xem lại code nào.

Đầu tiên chúng ta check coi số n có chia hết cho 3 không `n % 3 == 0`, sau đó check coi n có chia hết cho 5 không `n % 5 == 0`. À đúng rồi, vậy là nó hốt luôn cả 2 trường hợp chia hết cho 3 và 5 rồi, cho nên trường hợp cùng chia hết cho cả 3 và 5 thì không thể nào mà nằm ở dưới cùng như vậy được.

Vậy cách giải quyết là ngay từ đầu chúng ta phải kiểm tra trường hợp cùng chia hết cho 3 và 5 trước.

**FizzBuzz.rb**

```
def run(n)
  if n % 3 == 0 && n % 5 == 0
    return "FizzBuzz"
  elsif n % 3 == 0
    return "Fizz"
  elsif n % 5 == 0
    return "Buzz"
  end
end
```

Đó, làm vậy mới đúng, giờ chạy test lại coi nào:

```
# Running:

...

Finished in 0.001329s, 2257.3363 runs/s, 2257.3363 assertions/s.

3 runs, 3 assertions, 0 failures, 0 errors, 0 skips
```

Đó! Pass rồi thấy chưa? Vừa rồi không phải là tui gà đâu nha, tui cố tình sai cho mấy bạn khỏi bị mất tập trung đó :trollface: nhớ đó nha, không phải do tui gà đâu đó :chicken: 

### Test case #4: Nhận vào một số không chia hết cho 3 hay 5 gì cả, trả về chính số đó

Test case cuối cùng này thì nhẹ nhàng thôi, không có trap gì giống như khúc trên nữa đâu, ứ yên tâm mà test.

#### Viết test trước...

Trong trường hợp này, input có thể sẽ là 2, 4, 8 gì đó, miễn là nó không chia hết cho thằng nào trong 2 số 3 và 5 là được.

**FizzBuzz_test.rb**

```
def test_fizzbuzz_run_return_n
  expect = 8
  actual = FizzBuzz.run(8)
  assert_equal expect, actual
end
```

Test sẽ fail, nếu không fail thì cần phải xem lại :joy: 

```
# Running:

..F.

Finished in 0.001451s, 2756.7195 runs/s, 2756.7195 assertions/s.

  1) Failure:
FizzBuzzTest#test_fizzbuzz_run_return_n [/Users/huy/Desktop/Code/rbfizzbuzz/FizzBuzz_test.rb:26]:
Expected: 8
  Actual: nil

4 runs, 4 assertions, 1 failures, 0 errors, 0 skips
rake aborted!
```

#### ...Viết code sau

Rồi, giờ thì implement cho trường hợp cuối cùng, code nhanh còn đi cà phê chớ. Mình có cái tật đang code mà ai rủ rê đi cà phê uống nước là bực lắm, tại nghe rủ là đứng dậy đi liền à...

**FizzBuzz.rb**

```
def run(n)
  if n % 3 == 0 && n % 5 == 0
    return "FizzBuzz"
  elsif n % 3 == 0
    return "Fizz"
  elsif n % 5 == 0
    return "Buzz"
  else
  	return n
  end
end
```

Test luôn:

```
# Running:

....

Finished in 0.001397s, 2863.2785 runs/s, 2863.2785 assertions/s.

4 runs, 4 assertions, 0 failures, 0 errors, 0 skips
```

Toẹt zời! Pass hết luôn rồi!!!

## Kết luận

Đến đây, các bạn đã implement xong một module FizzBuzz hoàn chỉnh, với đầy đủ mọi test case cần thiết để đảm bảo module luôn chạy đúng và không xảy ra bug tiềm ẩn. Cho nên trong quá trình sử dụng module này trong dự án, nếu có xảy ra lỗi thì các bạn có thể chạy lại test để đảm bảo vấn đề không nằm trong này. Vậy là tiết kiệm được một chút thời gian khi debug.

Các bạn có thể tham khảo source đầy đủ của module FizzBuzz trong bài [tại đây](https://gist.github.com/huytd/db4624346a16c4510598e28602005f5f)

---

Để áp dụng TDD một cách hiệu quả thì bạn cần phải nắm rõ được yêu cầu, chia được vấn đề cần xử lý thành các vấn đề nhỏ hơn (từng test case), điều này đòi hỏi bạn phải tốn một khoản thời gian ban đầu để suy nghĩ về việc mình cần làm trước khi thực sự bắt tay vào code. Nhưng lợi ích mà nó đem lại thì rất nhiều, giống như việc bạn đi vào rừng mà không sợ bị lạc vì đã có sẵn tấm bản đồ ngồi tự vẽ từ đêm hôm qua vậy đó :laughing: 

Vậy thì, nên lựa chọn giữa tốc độ (bay vào code ào ào) hay chất lượng (uống miếng nước ăn miếng bánh, viết miếng test case trước rồi mới code)? Bỏ thời gian ra ngồi debug và cãi nhau với tester hay nhận bug về chạy test để biết được vấn đề nằm ở đâu rồi fix một cách nhanh chóng? Trở thành một lập trình viên chuyên nghiệp hay một công nhân fix bug, fix vài năm rồi lên làm leader :trollface:? Hy vọng đến đây các bạn đã có câu trả lời cho chính bản thân mình.

Happy bug fixing ^^
