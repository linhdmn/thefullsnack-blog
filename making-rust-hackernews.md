---
title: Viết ứng dụng đọc tin HackerNews bằng Rust
published: true
date: 2017-09-01 05:38:36
tags: rust, code
description: Dạo này mình toàn viết bài linh tinh, lâu rồi chưa thấy viết bài kĩ thuật nào mới nên hôm nay mình viết trở lại, mất công các bạn lại bảo mình không biết code =)))
image:
---
Dạo này mình toàn viết bài linh tinh, lâu rồi chưa thấy viết bài kĩ thuật nào mới nên hôm nay mình viết trở lại, mất công các bạn lại bảo mình không biết code =)))

Chủ đề lần này sẽ là: **Viết ứng dụng đọc tin trên HackerNews**, và chúng ta sẽ sử dụng **Rust**.

Qua bài này, các bạn sẽ được làm quen với rất nhiều kĩ thuật trong Rust như:

- Làm web với framework [rocket.rs](https://rocket.rs)
- Tạo và sử dụng module
- Parse nội dung RSS dùng crate [rss](https://crates.io/crates/rss)
- Chuyển đổi giữa các cấu trúc dữ liệu khác nhau
- Kiểm soát lỗi với kiểu dữ liệu `Result`
- Viết và chạy Unit Test với `cargo`

Giới thiệu vậy đủ rồi, giờ vô nội dung chính.

## Cụ thể là làm cái gì đây?

Chúng ta sẽ viết một ứng dụng đọc tin RSS từ feed của HackerNews tại địa chỉ [https://news.ycombinator.com/rss](https://news.ycombinator.com/rss). Việc đọc và parse nội dung RSS chúng ta sẽ sử dụng crate có tên là [rss](https://crates.io/crates/rss).

Sau đó, chúng ta sẽ viết một giao diện web đơn giản để hiển thị danh sách các mẫu tin đã parse được. Chúng ta sẽ sử dụng framework có tên là [rocket.rs](https://rocket.rs) cho phần này.

Lý do vì sao à? Tại vì HackerNews, như các bạn đã biết, thì giao diện của nó quá xấu. Tự nhận thấy mình có thể làm xấu hơn nên mình quyết định làm thôi.

## Rồi, giờ code luôn được chưa?

Được luôn. Lưu ý, nếu bạn chưa biết Rust là gì và chưa nắm được cách cài đặt, thì có thể tham khảo các bài viết sau trước khi chúng ta bắt đầu:

- [Rust là gì? có ăn được không](https://thefullsnack.com/posts/rust-intro.html)
- [Cách cài đặt Rust](https://thefullsnack.com/posts/rust-linux.html)

### Zeroend: Khởi tạo project

Vâng, mọi dự án luôn bắt đầu bằng cái bước setup project, không trật đi đâu được:

```
$ cargo new --bin hackernews-rs
```

Lệnh trên sử dụng `cargo` để tạo một binary project (nôm na là project chạy được - executable) có tên là `hackernews-rs`.

### Backend: Thu thập nội dung từ RSS

Chúng ta đi vào phần khó trước, phần dễ để dành tráng miệng lúc sau. Ông bà ta vẫn có câu **vạn sự khởi đầu nan** mà.

Nhưng mà đâu đó giữa trong giang hồ cũng tồn tại câu nói **gian nan bắt đầu nản**...

Mà mình thì không muốn các bạn nản, vì như vậy các bạn sẽ không đọc bài mình viết nữa :)) cho nên ở phần này, thay vì tự viết bộ parser, chúng ta sẽ sử dụng một crate có sẵn, là `rss`.

<div class="box-orange padding-15"><b>Crate</b> là tên gọi của các gois thư viện mở rộng dùng trong Rust. Giống như gem trong Ruby hoặc npm packages trong Node.js
</div>

#### Cài đặt crate `rss`

Để cài đặt crate này, chúng ta mở file `Cargo.toml` của project, tại đây bạn sẽ thấy phần khai báo `[dependencies]` đang bỏ trống. Add thêm một dòng vào ngay bên dưới, như sau:

**Cargo.toml**
```rust
[dependencies]
rss = { version = "*", features = ["from_url"] }
```

Như vậy chúng ta đã cho `cargo` biết rằng chúng ta sẽ add một crate tên là `rss` với phiên bản mới nhất (ký hiệu dấu `*`) vào project. Phần `features` là tùy chọn để sử dụng thêm chức năng `from_url` của crate này. Đối với các loại crate khác thì bạn không nhất thiết phải có phần này.

Trong thực tế, bạn nên chỉ định phiên bản cụ thể của crate khi muốn cài, để tránh việc các crate update thường xuyên kéo theo nhiều thay đổi, và bạn sẽ gặp bug phát sinh.

Trong file `main.rs`, chúng ta dùng từ khóa `extern crate` để import:

**main.rs**
```rust
extern crate rss;

fn main() {
  // Make it happen, or it will never happen.
}
```

#### Tạo module đọc tin

Chúng ta sẽ tạo ra một module tên là **fetch**, làm nhiệm vụ đọc và parse file RSS từ bên ngoài, trả về một mảng (hoặc một vector), có các phần tử là từng bản tin.

Đầu tiên, tạo một file mới trong thư mục `src` (cùng thư mục với `main.rs`), đặt tên là `fetch.rs`.

```
$ cd hackernews-rs
$ touch src/fetch.rs
```

Và gõ vào đoạn code sau:

**fetch.rs**
```rust
use super::rss;
use rss::{Channel, Item};

pub type FetchResult<T> = Result<T, rss::Error>;

pub fn fetch_from(url: &str) -> FetchResult<Vec<Item>> {
  Ok(Channel::from_url(url)?.items().to_vec())
}
```

Và khai báo `mod` ở trong `main.rs`:

**main.rs**
```rust
extern crate rss;

mod fetch;
use fetch::*;

fn main() {
  ...
```

Trong đoạn code trên, chúng ta sử dụng hàm `rss::Channel::from_url` của crate `rss`, hàm này có chức năng download gói tin RSS từ một URL bên ngoài, parse nó và trả về một đối tượng kiểu [`rss::Channel`](https://docs.rs/rss/0.7.0/rss/struct.Channel.html).

Mục đích của chúng ta là lấy ra danh sách các mẫu tin. Trong một đối tượng kiểu `rss::Channel`, chúng ta có hàm [`items()`](https://docs.rs/rss/0.7.0/rss/struct.Channel.html#method.items) để lấy các mẫu tin, hàm này trả về một `slice` (có thể hiểu là mảng), và chúng ta cần chuyển nó về dạng vector với hàm `to_vec()`.

<div class="box-orange padding-15"><b>Vector</b> là một mảng (array) không cần xác định trước kích thước, và có thể tăng giảm số lượng các phần tử tùy ý.
</div>

Kiểu `FetchResult<T>` ở đây là cách khai báo nhằm rút gọn cho cú pháp `Result<T, E>`.

Một đối tượng kiểu `Result<T, E>` sẽ trả về một trong 2 giá trị tùy vào từng trường hợp:

- Giá trị kiểu `T`: Trong trường hợp lệnh thực hiện thành công
- Giá trị lỗi kiểu `E`: Trong trường hợp xảy ra lỗi

Trong trường hợp này, `T` là một `Vec<Item>` như đã nói ở trên, `E` là `rss::Error`, là kiểu báo lỗi từ phía crate `rss`.

<div class="box-orange padding-15">
Các bạn có thể đọc thêm bài <a href="https://kipalog.com/posts/Ban-ve-cach-xu-ly-loi-cua-Rust">Bàn về cách xử lý lỗi của Rust</a> để hiểu thêm về kiểu dữ liệu này.
</div>

Bạn đã viết xong module **fetch**, giờ làm sao biết được nó có chạy hay không? Phải test!

#### Test kiểu chày cối

Với nhiều người, họ sẽ đơn giản là vào trong `main.rs` và gõ đoạn code kiểu như thế này:

**main.rs**
```rust
extern crate rss;

mod fetch;
use fetch::*;

fn main() {
  let result = fetch_from("https://thefullsnack.com/rss.xml");
  if result.is_ok() {
    println!("Yay! It's worked!");
  }
}
```

Chạy thử thấy màn hình in ra dòng chữ:

```
Yay! It's worked!
```

Vậy là yên tâm nó chạy được.

Nếu bạn định test như cách trên thì [xin đừng](http://www.nhaccuatui.com/bai-hat/xin-anh-dung-emily-ft-lil-knight-ft-justatee.wcDv6N6l4k.html)! Đây không phải là Unit Test, mà đây cũng không phải là cách để test, vì giờ nó chạy được, ai biết được ít hôm nữa bạn sửa code thì nó có còn chạy được không? Vì hàm `main` đâu phải là nơi để giữ đoạn code test này của bạn mãi mãi? Đúng hêm?

#### Viết test cases

Nếu bạn chưa biết viết test trong Rust như thế nào, có thể tham khảo bài [Cách viết test trong Rust the idiomatic way](https://thefullsnack.com/posts/rust-testing.html).

Đối với hàm `fetch_from` ở trên, chúng ta sẽ có tầm 3 đến 4 test cases, cụ thể là:

- **Fetch từ một link RSS hợp lệ có trả về kết quả hợp lệ hay không?** Để test case này, ta gọi hàm `fetch_from` với tham số là một liên kết RSS hợp lệ.
- **Fetch từ một link không tồn tại có trả về thông báo lỗi hay không?** Để test case này, ta truyền vào một URL không caa thật.
- **Fetch từ một link không phải là RSS thì có trả về thông báo lỗi hay không?** Để test case này, ta truyền vào một URL chứa nội dung không phải RSS, ví dụ như là một nội dung JSON.

Ở cuối file `fetch.rs`, ta thêm vào 3 hàm test từ 3 case ở trên:

**fetch.rs**
```rust
...

#[test]
fn test_fetch_from_valid_url() {
  let result = fetch_from("https://thefullsnack.com/rss.xml");
  assert!(result.is_ok());
  assert!(result.unwrap().len() > 0);
}

#[test]
fn test_fetch_from_invalid_url() {
  let result = fetch_from("https://where-superman-meet-wonderwoman.com/and-they-got-married.xml");
  assert!(result.is_err());
}

#[test]
fn test_fetch_from_invalid_rss_url() {
  let result = fetch_from("https://xkcd.com/info.0.json");
  assert!(result.is_err());
}
```

Chạy test với lệnh:

```
cargo test
```

Kết quả sẽ như thế này, cả 3 test case đều pass hết, chứng tỏ người code rất xịn:

```cpp
running 3 tests
test fetch::test_fetch_from_invalid_url ... ok
test fetch::test_fetch_from_invalid_rss_url ... ok
test fetch::test_fetch_from_valid_url ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Giờ thì chúng ta có thể đảm bảo module **fetch** hoạt động chính xác rồi.

#### Chuyển danh sách bản tin thành JSON

Để lấy danh sách các tin mới nhất từ **HackerNews**, chúng ta gọi hàm `fetch_from` với tham số là bản tin RSS của trang này:

```
let result = fetch_from("https://news.ycombinator.com/rss");
```

Việc tiếp theo, chúng ta sẽ viết phần frontend để hiển thị nội dung này ra trên nền web. Nhưng trước tiên, ta cần chuyển danh sách bản tin dạng `Vec<Item>` này thành nội dung định dạng JSON.

Việc chuyển đổi, chúng ta sẽ dùng một crate tên là `serde`. Để cài crate này, mở file `Cargo.toml` và thêm vào phần `[dependencies]`:

**Cargo.toml**
```
...

serde = "1.0.11"
serde_derive = "1.0.11"
serde_json = "1.0.2"
```

Trong `main.rs`, dùng từ khóa `extern crate` để import các package của `serde` vào:

```rust
extern crate serde;
#[macro_use] extern crate serde_derive;
#[macro_use] extern crate serde_json;
```

`#[macro_use]` ở đây gọi là `attribute`, và attribute này cho `cargo` biết chúng ta muốn sử dụng các `macro` được khai báo bên trong crate này.

<div class="box-orange padding-15"><b>Macro</b> là một chức năng của Rust cho phép bạn tạo ra các loại cú pháp tự chọn, giúp code tiện lợi hơn, có thể xem chi tiết <a href="https://doc.rust-lang.org/1.7.0/book/macros.html">tại đây</a>.
</div>

Kiểu [rss::Item](https://docs.rs/rss/0.5.1/rss/struct.Item.html) chứa rất nhiều trường mà chúng ta không cần dùng tới, trên thực tế, chúng ta chỉ cần các thông tin như `title`, `link`, `description` của một `Item`, vì thế, chúng ta nên tạo một kiểu dữ liệu khác để chức các thông tin cần trích xuất.

Cụ thể ở đây ta tạo một struct mới tên là `RSSItem`. Thêm đoạn code sau vào `fetch.rs`:

**fetch.rs**
```rust
#[derive(Serialize)]
pub struct RSSItem {
    pub title: String,
    pub link: String,
    pub description: String,
    pub pub_date: String,
}

impl From<Item> for RSSItem {
    fn from(item: Item) -> Self {
        RSSItem {
            title: item.title().unwrap_or_default().to_owned(),
            link: item.link().unwrap_or_default().to_owned(),
            description: item.description().unwrap_or_default().to_owned(),
            pub_date: item.pub_date().unwrap_or_default().to_owned(),
        }
    }
}
```

Trong đoạn code trên, ta tạo ra một struct tên là `RSSItem`, kế thừa thuộc tính `Serialize` của `serde`, để thư viện này dễ dàng chuyển đổi (convert) nó về định dạng JSON sau này.

Từ khóa `impl` dùng để implement một `trait` cho một kiểu dữ liệu bất kỳ, ở đây là chúng ta implement trait có tên là `From<T>` cho `RSSItem`.

<div class="box-orange padding-15">
<b>Trait</b> là một tập hợp các methods được định nghĩa sẵn. Có thể được impl (implement) cho một kiểu dữ liệu bất kỳ. Sau khi implement, kiểu dữ liệu này sẽ mang toàn bộ các methods của trait đó. Xem ví dụ chi tiết <a href="https://rustbyexample.com/trait.html">tại RustByExample</a>.
</div>

Trait `From<T>` thực hiện việc ép kiểu (casting) từ kiểu dữ liệu `T` sang kiểu dữ liệu được implement, mà ở đây chính là `RSSItem`. 

Tiếp theo, chúng ta thay đổi hàm `fetch_from` một tí, để trả về một vector kiểu `RSSItem` thay vì `rss::Item`:

**fetch.rs**
```rust
pub fn fetch_from(url: &str) -> FetchResult<Vec<RSSItem>> {
    Ok(Channel::from_url(url)?
                .items()
                .into_iter()
                .map(|item| RSSItem::from(item.clone()))
                .collect())
}
```

Ở đây chúng ta dùng hàm `into_iter()` để chuyển vector `rss::Item` về dạng có thể duyệt qua được. Sau đó dùng hàm `map()` để xử lý chuyển đổi từng phần tử kiểu `rss::Item` sang kiểu `RSSItem`, nhờ vào trait `From<T>` mà chúng ta đã implement ở trên.

Và cuối cùng, để đảm bảo rằng sau khi thay đổi, hàm `fetch_from` vẫn hoạt động bình thường, và cho kết quả đúng, ta thêm vào một test case mới:

**fetch.rs**
```rust
...

#[test]
fn test_fetch_is_convertable_to_json() {
    let items = fetch_from("https://thefullsnack.com/rss.xml");
    assert!(items.is_ok());
    let json_data = json!({ "items": items.unwrap() });
    assert!(json_data["items"].is_array());
    assert!(json_data["items"][0].is_object());
    assert!(json_data["items"][0]["title"].is_string());
}
```

Ở đây chúng ta sử dụng macro `json!()` của crate `serde_json` để chuyển một đối tượng thành một nội dung kiểu `serde_json::value::Value`. Các bạn thử đọc [document](https://docs.serde.rs/serde_json/value/enum.Value.html) của đối tượng này và tự suy luận xem vì sao lại viết hàm test ở trên như thế nhé.

Giờ chạy test thôi:

```cpp
running 4 tests
test fetch::test_fetch_invalid_url ... ok
test fetch::test_fetch_invalid_rss_feed ... ok
test fetch::test_fetch_is_convertable_to_json ... ok
test fetch::test_fetch_valid_rss_url ... ok

test result: ok. 4 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Như vậy có nghĩa là hàm `fetch_from` vẫn hoạt động tốt sau khi bị thay đổi, và đến đây thì có thể yên tâm bước qua giai đoạn tiếp theo được rồi.

### Frontend: Hiển thị bảng tin

Nào, bây giờ chúng ta bắt tay vào phần frontend thôi.

Chúng ta sẽ sử dụng một framework có tên là [rocker.rs](https://rocket.rs), đây là một framework gọn nhẹ, với chức năng chính là routing, tuy nhiên có thể mở rộng ra với _"rất nhiều chức năng phụ thêm, mà nếu không cần dùng thì bạn có thể không cần cài"_, đoạn in nghiên cũng là nguyên văn lời dụ dỗ của anh Sergio, tác giả Rocket.rs, nói với mình khi mình lên IRC hỏi tìm một web framework gọn nhẹ cho Rust.

<div class="box-orange padding-15">
Cộng đồng Rust quốc tế hoạt động rất mạnh trên các <a href="https://www.rust-lang.org/en-US/community.html#irc-channels">IRC channels</a>, với một lượng thành viên online thường trực rất đông đảo. Ngoài ra còn có hẳn một team từ Mozilla được trả lương để online và chat trên đó. Nên bất kì câu hỏi gì cũng được giải đáp ngay lập tức trong vòng chưa đầy 1 phút.
</div>

#### Cài đặt rocket.rs

Để cài đặt `rocket.rs`, chúng ta cài như khi cài một crate mới ở các bước trên.

**Cargo.toml**
```
[dependencies]
...

rocket = "0.3.0"
rocket_codegen = "0.3.0"

[dependencies.rocket_contrib]
default-features = false
features = ["handlebars_templates"]
```

Các crate cần cài cho framework này gồm có: `rocket`, `rocket_codegen` và `rocket_contrib`. Riêng gói `rocket_contrib` chúng ta chỉ dùng chức năng `handlebars_templates`.

#### Làm quen với rocket.rs

Tiếp theo, mở file `main.rs` và gõ vào đoạn code như sau:

**main.rs**
```rust
#![feature(plugin)]
#![plugin(rocket_codegen)]
extern crate rocket;
extern crate rocket_contrib;

#[get("/")]
fn index() -> &'static str {
    "Wheresoever you go, go with all your heart."
}

fn main() {
    rocket::ignite()
        .mount("/", routes![index])
        .launch();
}
```

Đoạn code trên tạo ra một web server đơn giản, có 1 route là `/`, được thiết lập bằng thuộc tính `#[get("/")]`, chính là trang chủ, và được handle bằng hàm `index()`, hàm này trả về một chuỗi `&str`.

<div class="box-orange padding-15">
Từ khóa <code>'static</code> đặt trong dòng khai báo <code>&'static str</code> được gọi là <b>lifetime</b> của một biến. Biểu diễn phạm vi tồn tại của một biến đó trong hàm. Trong trường hợp này biến đó mang kiểu <code>&str</code>.

Trong Rust, <code>'static</code> là lifetime dài nhất, và tồn tại cho đến khi chương trình kết thúc.
</div>

Bây giờ chúngta thử chạy server này bằng lệnh:

```
cargo run
```

Output hiện ra trên màn hình sẽ như thế này:

```
  Configured for development.
    => address: localhost
    => port: 8000
    => log: normal
    => workers: 8
    => secret key: generated
    => limits: forms = 32KiB
    => tls: disabled
  Mounting '/':
    => GET /
  Rocket has launched from http://localhost:8000
```

Khi truy cập vào địa chỉ `http://localhost:8000` bạn sẽ thấy in ra trên màn hình dòng chữ:

```text
Wheresoever you go, go with all your heart.
```
Vậy là chúng ta đã làm được một trang web đơn giản bằng `rocket.rs` rồi. Bây giờ chúng ta tiếp tục build ứng dụng HackerNews thôi.

#### Build HackerNews Frontend

Để cho đơn giản, thì ứng dụng của chúng ta chỉ cần 1 route duy nhất, đó là `/`, và ngay khi truy cập vào trang chủ thì chúng ta sẽ hiển thị danh sách các bản tin luôn.

Đầu tiên, chúng ta thay đổi hàm `main` một chút như sau:

**main.rs**
```rust
#![feature(plugin)]
#![plugin(rocket_codegen)]
extern crate rss;
extern crate rocket;
extern crate rocket_contrib;
extern crate serde;
#[macro_use] extern crate serde_derive;
#[macro_use] extern crate serde_json;

mod fetch;

use fetch::*;
use rocket_contrib::Template;

const RSS_URL: &str = "https://news.ycombinator.com/rss";

#[get("/")]
fn index() -> Template {
    let news = fetch_from(RSS_URL).ok().expect("Could not read RSS");
    Template::render("index", &news)
}

fn main() {
    rocket::ignite()
        .mount("/", routes![index])
        .attach(Template::fairing())
        .launch();
}
```

Trong đoạn code trên, ta khai báo một hằng `RSS_URL` để lưu địa chỉ RSS cần parse. Và sử dụng hàm `fetch_from` đã tạo ra trong module `fetch` để lấy về danh sách các mẫu tin.

Ta cũng sử dụng một cấu trúc dữ liệu mới là `rocket_contrib::Template`, khi một handler function trả về kiểu dữ liệu này, thì `rocket.rs` sẽ tự động render file template tương ứng ra màn hình.

Tiếp đến ta tạo file template. Ở trong thư mục `hackernews-rs`, tạo một thư mục tên `templates`

```
$ mkdir templates
$ cd templates
```

Trong thư mục này, ta tạo một file `.html.hbs`, là file template của `Handlerbars`:

```
$ touch index.html.hbs
```

Nội dung file `index.html.hbs` như sau:

**index.html.hbs**
```html
<html>
  <head>
    <title>Make HackerNews Great Again!</title>
    <style>
    </style>
  </head>
  <body>
    <ul>
      {{#each this }}
      <li class="item">
        <div class="title"><a href="{{ link }}">{{ title }}</a></div>
        <div class="description">{{{ description }}}</div>
        <div class="metadata">{{ pub_date }}</div>
      </li>
      {{/each}}
    </ul>
  </body>
</html>
```

Hàm `Template:render` thực hiện việc đọc file template (ở đây là `index.html.hbs`) đồng thời truyền vào một biến, gọi là **context**, từ đó ta có thể truy xuất biến nào thông qua đối tượng `this` trong file template.

Ở trong file `index.html.hbs` trên, ta có **context** chính là đối tượng **news**, là một Vector các bản tin `RSSItem`, truy xuất thông qua đối tượng `this`. Hàm `#each` có tác dụng duyệt qua từng phần tử của vector này và với mỗi phần tử, thì in đoạn nội dung bên trong (thẻ `<li>`) ra màn hình.

Đến đay bạn có thể chạy thử để xem kết quả, bằng lệnh:

```
cargo run
```

Khi truy cap vào địa chỉ `http://localhost:8000`, danh sách các mẫu tin từ HackerNews sẽ hiện ra. Tuy nhiên giao diện lúc này vẫn còn khá xấu. Ta có thể thay đổi file `index.html.hbs` để chỉnh sửa giao diện lại một tí:

**index.html.hbs**
```html
<html>
  <head>
    <title>Make HackerNews Great Again!</title>
    <link href="https://fonts.googleapis.com/css?family=PT+Sans" rel="stylesheet"> 
    <style>
    html {
      padding: 0; margin: 0;
    }
    body {
      font-family: 'PT Sans', sans-serif;
      font-size: 16px;
      line-height: 22px;
      display: flex;
      flex-direction: row;
      padding: 0; margin: 0;
    }

    ul {
      margin: 0; padding: 10px;
      list-style: none;
      counter-reset: news-item-counter;
      flex-basis: 400px;
    }
    
    ul li {
      padding: 10px 10px 10px 30px;
      position: relative;
      margin: 0;
      cursor: pointer;
    }

    ul li:before {
      content: counter(news-item-counter);
      counter-increment: news-item-counter;
      position: absolute;
      top: 10px; left: 0;
      font-size: 0.8em;
      text-align: center;
      line-height: 24px;
      width: 24px; height: 24px;
      background: #27ae60;
      color: #FFF;
      font-weight: bold;
      border-radius: 3px;
    }

    ul li a {
      text-decoration: none;
      font-weight: bold;
      color: #27ae60;
    }

    .metadata {
      font-size: 0.8em;
    }

    #frame {
      flex: 1;
      border: none;
      border-left: 1px solid #27ae60;
    }
    </style>
  </head>
  <script>
  const loadPage = (url) => {
    let frame = document.getElementById("frame");
    frame.src = url;
  }
  </script>
  <body>
    <ul>
      {{#each this }}
      <li class="item" onclick="loadPage('{{ link }}')">
        <div class="title"><a href="{{ link }}">{{ title }}</a></div>
        <div class="metadata">{{ pub_date }} - {{{ description }}}</div>
      </li>
      {{/each}}
    </ul>
    <iframe id="frame"></iframe>
  </body>
</html>
```

Ở đoạn HTML trên, chúng ta chia màn hình ra thành 2 phần, một phần bên trái hiển thị danh sách các mẫu tin và một phần bên phải hiển thị nội dung của mẫu tin đó (sử dụng `iframe`). Giao diện sẽ như thế này:

![](img/rust-hackernews.png)

Vậy là chúng ta đã hoàn thành ứng dụng đọc tin HackerNews rồi :D

---

Giải pháp dùng `iframe` này chưa thực sự tối ưu, và vẫn còn khá nhiều việc cần phải làm như là loading bar hay handle lỗi khi không thể load được nội dung bài báo vào `iframe`.

Tuy nhiên sứ mạng của bài viết này - đó là hướng dẫn làm một ứng dụng web từ A-Z bằng Rust - thì đã kết thúc, cho nên mình sẽ phủi tay và việc fix bug, tối ưu hóa mình sẽ không viết tiếp nữa (dài quá lười thôi, không có gì đâu).
