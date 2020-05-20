# rake bower:install trong Docker

Nếu xài Docker làm môi trường dev khi làm việc với Rails, hẳn sẽ có lục bạn gặp lỗi sau khi chạy `rake bower:install`:

```
bower ESUDO Cannot be run with sudo
```

Nguyên nhân là vì `bower` không "thích" chạy với quyền root vì sẽ dễ dẫn đến các vấn đề về quyền truy cập về sau (lỗi mà dân xài `npm` rất thường gặp). Nhưng ngặt nỗi mặc định Docker cung cấp cho ta quyền root khi làm việc trong một container.

Cách giải quyết có rất nhiều, ví dụ như tạo user mới trong container đó để chạy, nhưng như vậy rất phiền phức. Giải pháp dễ hơn là sử dụng tham số `--allow-root` để bower bỏ qua bước kiểm tra quyền root này.

Có 2 cách để sử dụng tham số `--allow-root`:

### Thêm tham số trực tiếp

Bạn có thể chạy `rake` với tham số này trực tiếp như sau:

```
rake bower:install['--allow-root']
```

### Thêm tham số vào file .bowerrc

Cách 2 là tạo file `~/.bowerrc` với nội dung như sau:

```
{
   "allow_root": true
}
```

Rồi chạy `rake` như bình thường:

```
rake bower:install
```

huytd 10-09-2016
