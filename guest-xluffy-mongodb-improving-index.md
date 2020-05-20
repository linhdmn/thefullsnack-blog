---
title: Tối ưu hiệu suất MongoDB bằng cách quản lý index
published: guest
date: 2018-06-01 10:26:25
tags: guest, database, mongodb
description: Ai cũng biết đánh index giúp tăng tốc các truy vấn tìm kiếm, tìm kiếm trong cây index (B-Tree) sẽ nhanh hơn scan trong toàn bộ bảng. Ai cũng biết nên không nói nhiều về index là gì nữa...
image:
---

[Khách mời](/tags/guest.html) tuần này là anh "Quăng" ([@xluffy](https://github.com/xluffy/)), một nhân vật mà mình chả có gì để mà giới thiệu =))) anh hiện đang là devops ở đâu không rõ, ngoài ra anh ấy còn là thành viên của nhóm archlinuxvn, chắc nhiều bạn cũng biết, là nhóm này cũng không có mấy người biết tới :joy: jk

Các bạn có thể xem thêm bài viết gốc của tác giả tại [blog @xluffy](https://xluffy.github.io/post/improving-mongodb-perf-by-managing-indexes/).

---

Ai cũng biết đánh index giúp tăng tốc các truy vấn tìm kiếm, tìm kiếm trong cây index (B-Tree) sẽ nhanh hơn scan trong toàn bộ bảng. Ai cũng biết nên không nói nhiều về index là gì nữa :v vậy nhé.

Đây là [MongoDB users dataset](https://github.com/xluffy/assets/blob/master/users.bson.gz) bạn có thể tải về chơi thử. 

## 1. Sơ sơ về index

Index trong Mongo cũng tương tự các cơ sở dữ liệu quan hệ, ví dụ ta có một index được định nghĩa như sau:

```
db.users.createIndex({
  user_status: 1
}, {
  background: true,
  name: 'idx_users_by_user_status'
});

```

Câu trên sẽ nói với database build một index dựa trên giá trị của field `user_status` của collection `users`. 

Ngoài ra nó có thêm một tùy chọn là `backgroud: true` để build index ở dạng backgroud, thông thường đánh index sẽ block tất cả các operation khác, và với các collection có kích thước lớn thì việc đánh index sẽ tốn tới vài giờ để hoàn thành và trong thời gian này database sẽ không thể response, đánh index `background` để tránh tính trạng block các operation khác [nhưng không hẳn đời lúc nào cũng như mơ].

Lưu ý là nên chạy query đánh index trong `tmux` để tránh rớt kết nối và để kiểm tra process của index ta có thể truy vấn:

```
db.currentOp(
  { "msg": /Index Build/ }
);

"msg" : "Index Build (background) Index Build (background): 3305028/574961784 0%"
```

Ok, tiếp tục với index phía trên, nó sẽ có tác dụng với các truy vấn tìm kiếm với điều kiện là field `user_status`, ví dụ như sau:

```
db.users.find({
  "user_status": "new_user"
});
```

Không có gì phải bàn cãi thêm, truy vấn trên sử dụng index `idx_users_by_user_status` và trả về kết quả với tốc độ của một vị thần gió. :triumph:

Giờ giả sử, ta có một truy vấn lấy ra tất cả các user `confirmed` NHƯNG trong một tháng của năm 2013 như sau:

```
var start = ISODate("2013-04-01T00:00:00Z");
var end = ISODate("2013-04-30T23:59:59Z");
db.users.find({
    "user_status": "confirmed",
    "created_at": { $gte: start, $lte: end}
});
```

Tương tự như trên, lấy ra danh sách các `confirmed` khá nhanh chóng do sử dụng index như câu truy vấn trên cùng, tuy nhiên trong danh sách trả ra `confirmed` ta cần lọc thêm một lần nữa để lấy ra các user đăng kí trong tháng đó, và do `created_at` không được đánh index nên có bao nhiêu `confirmed` thì ta cần quét toàn bộ từng đấy.

Để giải quyết truy vấn trên, ta cần đánh index trên 2 field, gọi là **compound index** như sau:

```
db.users.createIndex({
  user_status: 1,
  created_at: 1 
}, {
  background: true, 
  name: 'idx_users_by_user_status_and_created_at' 
});
```

Ok, trong có vẻ ngon rồi, truy vấn của chúng ta đã nhanh hơn rồi. Nhưng thử nghĩ xem, chúng ta có thể làm tốt hơn hay không nhỉ?

Giả sử bạn có 20 triệu register user (hoặc bạn có thể tưởng tượng bạn có 1 tỷ register user và bạn giàu hơn Mark Zuckerberg :slightly_smiling_face: cũng được) `user_status` có các giá trị sau:

  - new_user
  - confirmed
  - banned
  - deleted

Nếu không có index trên `user_status`, bạn cần quét hết 20 triệu docs này, đánh index trên `user_status` con số giảm từ 20 triệu -> 5 triệu (1/4).

Giờ giả sử, start-úp mặt của bạn ra đời năm 2015, đến bây giờ là 3 năm, giả sử user đăng kí đồng đều giữa các ngày (thực tế thì đéo phải như vậy đâu  :sweat_smile:). Bạn index lại như sau:

```
db.users.createIndex({
  created_at: 1,
  user_status: 1
}, {
  background: true,
  name: 'idx_users_by_created_at_user_status'
});
```

Giờ với câu truy vấn trên, ta được một danh sách các user đăng ký trong vòng 1 tháng, là cỡ hơn 500 ngàn user (1 ngày có ~ 18 ngàn user đăng ký), sau đó lọc theo điều kiện `new_user` ta chỉ cần quét trong tập 500k user này so với 5 triệu như cách đánh index ban đầu.

Chốt lại, với **compound index** nên đánh index field có giá trị uniqe nhiều hơn trước. Ở trên thì đánh `created_at` trước vì các giá trị trong field này hầu hết đều khác nhau (trong khi `user_status` chỉ có 4 giá trị), nên sẽ giảm được vùng tìm kiếm của truy vấn nhiều hơn.

## 2. How to improve

Xong xuôi việc đánh index, bây giờ làm cách nào để ta có thể chắc chắn rằng database sẽ sử dụng index một cách hiệu quả? Index cũng như dữ liệu, được lưu trữ trên đĩa cứng, và để index có thể được sử dụng hiệu quả thì tốt nhất là nó nên được đặt trên RAM. Trong Mongo, RAM thì thường được sử dụng để giữ các dữ liệu và index thường xuyên truy vấn (WiredTiger internal cache) tránh phải đọc đĩa thường xuyên. Recommend là 50% physical memory, ví dụ server có 32GB thì d internal cache là 15GB.

Nhưng data thì lúc nào cũng lớn hơn RAM, bạn không thể đặt toàn bộ dữ liệu lên Wiredtiger cadhe được, với index cũng vậy, đánh index thì các truy vấn đọc dữ liệu trên điều kiện sẽ nhanh, nhưng quá nhiều index dư thừa thì gây ra 2 vấn đề:

- Không thể chứa tất cả các index trên memory.
- Làm chậm các truy vấn ghi dữ liệu như Insert/Update/Delete -> nhưng thực ra 

nếu phần ghi chiếm tới 80-95% thì việc đánh đổi cũng đáng kể.

Bạn có thể kiểm tra kích thước tổng thể của index trên cơ sở dữ liệu như sau:

```
db.runCommand({ dbStats: 1, scale: 1 });
{
    "db" : "test",
    "collections" : 2,
    "objects" : 2000000,
    "avgObjSize" : 96.120665,
    "dataSize" : 192241330,
    "storageSize" : 374370304,
    "numExtents" : 0,
    "indexes" : 6,
    "indexSize" : 53964800,
    "ok" : 1
}
```

Cụ thể hơn (dữ liệu này ăn cắp trên production)

```
db.stats().indexSize

86508650496
```

=> kích thước của tất cả các index trên CSDL là 86GB, có nghĩa là index không thể fit hết trên memory, nên khi nào cần sử dụng tới index, nếu index đó không có sẵn trên memory thì cần đọc index đó từ đĩa lên.

Nói chung, xác định bao nhiêu memory là cần thiết không dễ, bạn có thể trả lời vài câu hỏi sau để tự xác định và điều chỉnh memory cho hợp lý:

- Độ lớn data của bạn là bao nhiêu?
- Độ lớn của index là bao nhiêu?
- Độ phát triển của dữ liệu (trong 1 tháng, 1 năm?)
- Độ lớn của tập dữ liệu thường xuyên truy cập nhất (gọi là working set)?

Vậy chiến lược để giữ index có kích thước nhỏ là như thế nào?

### 2.1 Xóa các index không sử dụng

```
db.users.aggregate([ { $indexStats: {} } ]);

[{
    "name": "idx_users_by_created_at_user_status",
    "key": {
        "created_at": 1,
        "user_status": 1
    },
    "host": "6b1b716ae456:27017",
    "accesses": {
        "ops": NumberLong(456550227),
        "since": ISODate("2018-05-31T15:31:09.020Z")
    }
} {
    "name": "idx_users_by_user_status",
    "key": {
        "user_status": 1
    },
    "host": "6b1b716ae456:27017",
    "accesses": {
        "ops": NumberLong(277641131),
        "since": ISODate("2018-05-31T15:05:39.148Z")
    }
} {
    "name": "_id_",
    "key": {
        "_id": 1
    },
    "host": "6b1b716ae456:27017",
    "accesses": {
        "ops": NumberLong(0),
        "since": ISODate("2018-05-31T15:03:12.197Z")
    }
}]
``` 

Ở ví dụ trên, ta có 3 index, ở giá trị `accesses`, ta thấy 2 index đầu tiên được sử dụng rất nhiều lần, trong khi đó index thứ 3 không hề được sử dụng. Con số bao nhiêu là hợp lí để loại bỏ một index tùy thuộc vào số lượng truy vấn và nghiệp vụ của chính bạn, bạn có thể tự cân nhắc, đo lường và loại bỏ nếu không cần thiết.

Cần lưu ý, `ops` có thể bị reset, con số hiện tại thể hiện số lần được sử dụng kể từ `since` -> thời gian mongod được restart.

### 2.2 Loại bỏ các index dư thừa

```
db.users.aggregate([ { $indexStats: {} } ])

[{
    "name": "idx_users_by_user_status_created_at",
    "key": {
        "user_status": 1,
        "created_at": 1
    },
    "host": "6b1b716ae456:27017",
    "accesses": {
        "ops": NumberLong(456550227),
        "since": ISODate("2018-05-31T15:31:09.020Z")
    }
} {
    "name": "idx_users_by_user_status",
    "key": {
        "user_status": 1
    },
    "host": "6b1b716ae456:27017",
    "accesses": {
        "ops": NumberLong(277641131),
        "since": ISODate("2018-05-31T15:05:39.148Z")
    }
}]
```

Ở ví dụ trên, cả 2 index đều được sử dụng. Tuy nhiên index đầu tiên có thể làm cho index thứ 2 trở lên dư thừa, vì các truy vấn chỉ trên điều kiện `user_status` có thể sử dụng index đầu tiên mà không có vấn đề gì.

### 2.3 Sử dụng sparse index

Đây là kiểu đánh index trên điều kiện, ví dụ ta có 20 triệu user, nhưng nếu ta chỉ thường truy vấn `user_status` là `new_user` thì ta có thể đánh index riêng với tập `new_user` thôi -> giả sử số lượng `new_user` là 40% trên tổng số user -> index của chúng ta sẽ giảm xuống tới 60% kích thước.

### 2.4 Giảm bớt kích thước của collection

Cách duy nhất để giảm kích thước của collection đó là ... **xóa dữ liệu**, thực tế có những nghiệp vụ lưu trữ logs, events không cần giữ quá 1-2 năm để tra cứu thì ta có thể xóa bớt các dữ liệu không còn cần thiết (hoặc di chuyển nó sang một CSDL khác với tần suất tuy vấn thấp hơn). Khi kích thước của collection giảm, kích thước của index cũng sẽ giảm theo và data/index có thể fit vừa trên RAM.

### 2.4 Giữ index đơn giản

Compound index rất lợi hại trong trường hợp truy vấn dữ liệu trên nhiều điều kiện khác nhau, tuy nhiên nó cũng tốn chi phí bảo trì và dữ liệu càng lớn thì index size sẽ càng tăng nhanh. Tương tự như truy vấn, cố gắng giữ cho index một cách đơn giản nhất có thể.

## 3. Bonus

Đáng ra đéo có phần này, [nhưng sợ anh em lại bảo thằng chuyên bài trừ Mongo mà nay lại viết về Mongo](http://whyyoushouldusemongodb.com), thấy sai sai thế éo nào nên phải viết thêm tí.

Thật ra những lý thuyết trên về index và quản lý index đều có thể áp dụng cho các cơ sở dữ liệu quan hệ như MySQL hay PostgreSQL. Ví dụ với PostgreSQL cũng có index trên điều kiện đó là partial index tương tự như sparse index (MySQL thì không biết có không).

Bản chất, nếu để ý sẽ thấy các hệ CSDL có rất nhiều đặc điểm giống nhau (kể cả M$ SQL), lý thuyết này có thể áp dụng cho CSDL khác, và ngược lại. Cốt lõi của vấn đề là **tư duy** về việc **đo lường**, cách thức đo lường và đánh giá hiệu quả của mỗi hành động tác động lên hệ thống

Lần sau sẽ viết thêm về một số thứ liên quan đến metric trong CSDL quan hệ, hỗ trợ cho việc tối ưu hệ thống tương tự như trên MongoDB.

## 4. Ref

- https://mixmax.com/blog/improving-mongo-performance-by-managing-indexes
- https://github.com/ozlerhakan/mongodb-json-files
- https://docs.mongodb.com/manual/tutorial/ensure-indexes-fit-ram
