---
title: GPU programming với Golang
published: true
date: 2016-10-31 14:38:00
tags: gpu, golang, algorithm, math
description: 
image:
---
Ở [bài trước](https://thefullsnack.com/posts/nhan-ma-tran-2.html) mình có giới thiệu về kĩ thuật lập trình GPU với **OpenCL** bằng **C/C++**. Hôm nay mình sẽ giới thiệu tiếp kĩ thuật này trên Go.

![](https://archive.fosdem.org/2014/schedule/event/hpc_devroom_go/hpc_devroom_go-99c7a0bca25373a544f8d99c7e42526e04c7578bcfa3c45fc3be59fa1ada99ba.png)

## Sử dụng C/C++ trong Go

Một đặc điểm của Golang là chúng ta có thể thoải mái import các thư viện C/C++ và biên dịch bằng sự hỗ trợ của **Cgo**, các bác có thể xem lại bài [Một số kinh nghiệm làm việc với Cgo](https://thefullsnack.com/posts/vai-dieu-ve-cgo.html) để biết thêm chi tiết.

Về cách sử dụng thì chúng ta chỉ đơn giản là viết đoạn code **C/C++** trong phần comment đầu file và biên dịch:

```
package main

/*
#include <stdio.h>

void sayHello() {
  printf("YOLO!")
}
*/
import "C"

func main() {
  C.sayHello()
}
```

Cho nên chúng ta hoàn toàn có thể include thư viện **OpenCL** từ bên phía **C/C++** vào Go để chạy bằng Cgo. Tất nhiên phải chỉ định framework cần dùng ở đầu chương trình luôn, ví dụ:

```
/*
#cgo CFLAGS: -Wall
#cgo LDFLAGS: -framework opencl
#include <OpenCL/opencl.h>
*/
```

Hoặc với **CUDA** như sau:

```
//#include <cuda.h>
//#cgo LDFLAGS: -lcuda
```

Và viết code cho **Host Program** như bình thường.

## Kernel Program

Bạn có thể lập trình cho **Host Program** bằng Golang, tuy nhiên, đối với **Kernel Program** thì nó vẫn là một file `.cl` và nó vẫn phải dùng cú pháp của **C**, hoàn toàn không có sự thay đổi nào ở đây cả.

## Sử dụng Go Wrappers

Nếu cảm thấy việc khai báo cú pháp theo **Cgo** là hơi phức tạp và gây rối cho chương trình thì bạn có thể sử dụng các wrapper có sẵn mà cộng đồng Golang đã xây dựng, bản chất các wrapper này vẫn là sử dụng **Cgo** không có gì khác, chúng chỉ cung cấp cho bạn cú pháp dễ nhìn hơn và dễ build hơn mà thôi.

### CUDA Wrapper cho Go

Bài viết này không có ý định tìm hiểu sâu hơn về **CUDA** cho nên mình sẽ dẫn link để các bạn tham khảo. 

Tác giả Arne Vansteenkiste có viết một thư viện tên là [$mumax^{3}$](http://mumax.github.io/index.html) hỗ trợ lập trình GPU bằng **CUDA**, các bạn không nhất thiết phải sử dụng **MuMax** tuy nhiên có thể sử dụng **CUDA** wrapper mà **MuMax** cung cấp.

```
package main

import "github.com/mumax/3/cuda"

func main(){
  N := 3
  a := cuda.NewSlice(N)
  b := cuda.NewSlice(N)
  c := cuda.NewSlice(N)
  defer a.Free()
  defer b.Free()
  defer c.Free()

  a.CopyHtoD([]float32{0, -1, -2})
  b.CopyHtoD([]float32{0, 1, 4})

  cfg := Make1DConfig(N)
  add_kernel(a.Ptr(), b.Ptr(), c.Ptr(), cfg)

  fmt.Println("result:", a.HostCopy())
}
```

Ngoài ra các bạn có thể tham khảo thêm slide [**Scientific GPU computing with Go**](https://hpcugent.github.io/easybuild/files/FOSDEM14/FOSDEM14_HPC_devroom_14_GoCUDA.pdf) để biết thêm chi tiết về framework này.

### OpenCL

Với **OpenCL** thì chúng ta cũng có kha khá là nhiều các wrapper, tuy nhiên ở đây mình chọn sử dụng [**go-gl/cl**](https://github.com/go-gl/cl) vì dự án này được phát triển khá là nghiêm túc và rất active, tài liệu + community đầy đủ. 

Cách sử dụng thì rất đơn giản, đầu tiên bạn chỉ cần cài đặt gói **go-gl/cl** mới nhất, hoặc là bản stable nhất (là bản v1.2):

```
go get github.com/go-gl/cl/v1.2/cl
```

Hoặc có thể download và compile lại gói này với lệnh sau để đạt performance tốt hơn:

```
go install -gcflags="-l -l -l -l" github.com/go-gl/cl/v1.2/cl
```

Lệnh trên sẽ inline hầu hết mọi function trong package **go-gl/cl** và vì việc gọi function thì rất tốn kém ([xem ở đây](http://dave.cheney.net/2014/06/07/five-things-that-make-go-fast)), nên inline sẽ giúp giảm thiểu chi phí cho vụ này, tuy nhiên nhược điểm của cách này là dung lượng binary khi build sẽ lớn hơn nhiều.

Cách dùng thì chỉ cần import thư viện trên vào code, và gọi hàm thông qua đối tượng `cl`:

```
import "github.com/go-gl/cl/v1.2/cl"
...
cl.CreateKernel(program, cl.Str("hello"), errptr)
```

Các bạn có thể xem qua đoạn chương trình mẫu [tính bình phương một số](https://github.com/go-gl/cl/blob/master/sample/square/square.go) để hiểu thêm về cách dùng, cũng tương tự như với C/C++:

Mình lượt bỏ các phần linh tinh và ghi lại các phần chính cần lưu ý trong chương trình:

Import thư viện **go-gl/cl**:

```
package main

import "github.com/go-gl/cl/v1.2/cl"
```

Khai báo và biên dịch **Kernel**:

```
var KernelSource = `
__kernel void square(
   __global float* input,
   __global float* output,
   const unsigned int count)
{
   int i = get_global_id(0);
   if(i < count)
	   output[i] = input[i] * input[i];
}` + "\x00"

...


// Đọc và biên dịch kernel program
srcptr := cl.Str(KernelSource)
program := cl.CreateProgramWithSource(context, 1, &srcptr, nil, errptr)
defer cl.ReleaseProgram(program)

err = cl.BuildProgram(program, 1, &device, nil, nil, nil)
```

Lấy thông tin **Compute Device**:

```
err := cl.GetDeviceIDs(nil, cl.DEVICE_TYPE_GPU, 1, &device, nil)
```

Khởi tạo **Context**:

```
context := cl.CreateContext(nil, 1, &device, nil, nil, errptr)
defer cl.ReleaseContext(context)
```

Khởi tạo **Command Queue**:

```
cq := cl.CreateCommandQueue(context, device, 0, errptr)
defer cl.ReleaseCommandQueue(cq)
```
  
Khởi tạo **Kernel Object**:

```
kernel := cl.CreateKernel(program, cl.Str("square"+"\x00"), errptr)
defer cl.ReleaseKernel(kernel)
```

Khởi tạo **Memory Object**:

```
input := cl.CreateBuffer(context, cl.MEM_READ_ONLY, 4*DataSize, nil, errptr)
defer cl.ReleaseMemObject(input)

output := cl.CreateBuffer(context, cl.MEM_WRITE_ONLY, 4*DataSize, nil, errptr)
defer cl.ReleaseMemObject(output)
```

Ghi dữ liệu vào **Memory Object** trong **Device**:

```
err = cl.EnqueueWriteBuffer(cq, input, cl.TRUE, 0, 4*DataSize, unsafe.Pointer(&data[0]), 0, nil, nil)
err = cl.SetKernelArg(kernel, 0, 8, unsafe.Pointer(&input))
err = cl.SetKernelArg(kernel, 1, 8, unsafe.Pointer(&output))
err = cl.SetKernelArg(kernel, 2, 4, unsafe.Pointer(&count))
```

Khởi tạo **Work Group** để bắt đầu chạy **Kernel**: 

```
err = cl.GetKernelWorkGroupInfo(kernel, device, cl.KERNEL_WORK_GROUP_SIZE, 8, unsafe.Pointer(&local), nil)
err = cl.EnqueueNDRangeKernel(cq, kernel, 1, nil, &global, &local, 0, nil, nil)
cl.Finish(cq)
```

Đọc dữ liệu ra bằng **Channel**:

```
results := make([]float32, DataSize)
err = cl.EnqueueReadBuffer(cq, output, cl.TRUE, 0, 4*1024, unsafe.Pointer(&results[0]), 0, nil, nil)
```

Ở chương trình trên thì có sự xuất hiện của kí tự `\x00` ở phần `kernel source`, đây là kí tự [NULL](https://en.m.wikipedia.org/wiki/Null_character) dùng để báo hiệu kết thúc string. 

Và chúng ta cũng thấy, ở đây ta có thể tận dụng luôn các chức năng mà Go cung cấp một cách rất hiệu quả như là `defer` hoặc `channel`,...
