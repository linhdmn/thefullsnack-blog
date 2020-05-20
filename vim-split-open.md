# Mở nhiều file và tự động split trong Vim

Thông thường, khi mở nhiều file trong Vim, ta có thể dùng lệnh:

```
vim <file-1> <file-2> ...
```

Tuy nhiên với cách này, Vim chỉ hiện lên màn hình 1 file duy nhất và để chuyển qua lại giữa các file ta phải dùng lệnh `:next` hoặc `:previous` khá là phiền phức.

Bằng cách truyền thêm tham số `-o` (mở với các split nằm ngang) hoặc `-O` (mở với các split nằm dọc) tiện hơn rất nhiều.

Ví dụ:

```
vim -o <file-1> <file-2>

vim -O <file-3> <file-4>
```

Ta cũng có thể áp dụng lệnh này với Git, ví dụ, để mở các file đã sửa của commit mới nhất, ta gõ lệnh:

```
vim -O $(git diff --name-only HEAD~1)
```

Hoặc tương tự như trên nhưng chỉ mở các file `*.js`:

```
vim -O $(git diff --name-only HEAD~1 | awk '/\.js/{print}')
```
