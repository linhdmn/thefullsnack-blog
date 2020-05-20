---
title: Bảo mật 2 lớp và ứng dụng
published: guest
date: 2018-05-25 21:14:15
tags: guest, golang
description: 
image:
---
[Khách mời](/tags/guest.html) tuần này của chúng ta là một bạn trai giấu tên, có tên là [Runi](http://runikitkat.com/). Trong bài này, tác giả giới thiệu cho cho chúng ta những khái niệm cơ bản về bảo mật 2 lớp (Two Factor Authentication - 2FA), và cách tích hợp 2FA vào hệ thống sẵn có, với code minh họa bằng Golang.

Bài viết được đăng lại với sự đồng ý của tác giả, các bạn cũng có thể [xem bài viết gốc tại đây](http://runikitkat.com/post/2fa/).

---

Trong thế giới của việc thông tin cá nhân đang ngày càng bị xâm phạm và tấn công, đến thời điểm này đã có gần [10 tỷ](https://breachlevelindex.com/) accounts bị leaked thì chuyện phải implement một phương pháp đăng nhập có tính bảo mật cao hơn là một chuyện bất cứ cá nhân/tổ chức nào cũng aware được.

Two-factor Authentication là một trong số đó. Trong bài này mình sẽ chia sẻ những kiến thức đã tổng hợp được trong quá trình làm việc với 2FA.

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/7gklibkwls_image.png)

## Two-Factor Authentication (2FA) là gì?

Two-Factor Authentication (2FA) hay còn gọi là Bảo mật 2 lớp, là một phương thức để chứng thực user bằng việc combine 2 factors khác nhau từ bộ source:

- Một cái gì đó bạn biết.

- Một cái gì đó bạn có.

- Một cái gì đó mà nó mặc nhiên như vậy.

Một ví dụ khá cơ bản của 2FA đó là khi bạn rút tiền ở ATM, bạn cần 2 thứ, một là cái thẻ (bạn có) và mã PIN (bạn biết) mới có khả năng thực hiện rút tiền.

Như vậy nó áp dụng trong online authentication như thế nào?

Với bảo mật thông thường, bạn chỉ cần nhập username và password để đăng nhập tài khoản của mình, thứ bảo vệ duy nhất của bạn là mật khẩu. Đây là cái mà "bạn biết". 2FA sẽ thêm một extra layer để bạn cần provide cái mà "bạn có" nữa.

## Các loại 2FA

- Hardware Token

- SMS/Voice based

- Software Token

- Push notification

- Các loại khác

Như bạn thấy, có rất nhiều cách để mô phỏng cái mà "bạn có". 

Chẳng hạn, SMS là một sự lựa chọn không tồi, server gửi SMS về cho user để đăng nhập. Nhưng SMS lại ko quá an toàn (có thể bị intercepted), và bị vấn đề về network, việc delay có thể làm ảnh hưởng tới authentication process.

Vì nhiều lí do khác nhau nên team quyết định chọn dùng Software Token cho 2FA. Và từ bây giờ trong bài viết mình 2FA cũng được ám chỉ tới phương pháp sử dụng cryptographically key của Software Token.

## 2FA hoạt động như thế nào?

Mình lấy ví dụ gitlab.

Đây là lúc đăng nhập, mình sẽ provide user và password.

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/qg21xb61c9_Screen%20Shot%202018-05-25%20at%203.15.24%20PM.png)

Sau khi đăng nhập xong, vì mình có enable 2FA nên nó sẽ redirect mình tới trang điền code (soft token).

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/wi0hndhayn_Screen%20Shot%202018-05-25%20at%203.15.33%20PM.png)

Code này sẽ được lấy từ Authentication application, có thể là Authy, Google authenticator ...

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/mui8qfsb9c_Screen%20Shot%202018-05-25%20at%204.43.50%20PM.png)

Ví dụ như mình dùng Authy để lấy được code. Mỗi code sẽ được thay đổi sau 30s.

Với code này mình sẽ đăng nhập thành công.

## Cơ chế hoạt động internally của 2FA

Khi bạn enable 2FA cho tài khoản của mình, bạn sẽ nhận được một secret key based 32. Tùy vào mức độ security, độ dài của secret key có thể là 80, 128 hoặc 160 bit.

Các authenticator application sẽ scan secret này dưới dạng QR code (hoặc manually) và dùng nó để generate ra một HMAC-SHA1. Chuỗi HMAC này có thể là một trong 2 dạng:

- [TOTP](https://en.wikipedia.org/wiki/Time-based_One-time_Password_algorithm) 

- [HOTP](https://en.wikipedia.org/wiki/HMAC-based_One-time_Password_algorithm)

Sau đó HMAC này sẽ được extracted và lấy ra 1 số int 4 byte, đó chính là code.

Một mã code sẽ valid trong 30 giây. Tuy nhiên không phải ai cũng có clock synced giống nhau, vì network latency các kiểu nên thường mọi người hay cho phép ở phạm vi cộng trừ 1 code, tức là 1 code sẽ valid trong 1 phút 30 giây. Điều này có thể giảm tính an toàn, nhưng lại tăng sự trải nghiệm đáng kể.

### Backup codes

Backup codes hay recovery codes sẽ được sử dụng trong trường hợp bạn không thể sử dụng điện thoại, bạn có thể dùng chúng để đăng nhập. Có 2 loại backup codes:

- Multiple backup codes: Ví dụ github sẽ cho bạn 10 codes, và mỗi code sẽ được sử dụng 1 lần.

- Single backup code: Bạn dùng cái code này đồng nghĩa với việc bạn có thể đăng nhập, nhưng phải setup lại 2FA (nó assume bạn bị mất điện thoại).

## Làm thế nào để apply 2FA vào dự án của mình?

Khá đơn giản, hầu hết ngôn ngữ đều open source implementation của mình. Mình code Go, library mà mình chọn là `github.com/dgryski/dgoogauth` được viết bởi Damian Gryski, một gopher rất nổi tiếng.

Flow chương trình 

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/hnxlni0dqr_Screen%20Shot%202018-05-25%20at%206.07.49%20PM.png)

Khi một user muốn enable 2FA, App sẽ gọi `RequestGenerate2FA` API. 

Trong API này sẽ generate ra một temporary secret và lưu vào redis, trong thời gian expire 5 phút.

GenerateSecret thì khá đơn giản thôi.

```go

// GenerateSecret generates a 10 byte secret string.

func GenerateSecret() (string, error) {

    random := make([]byte, 10)

    _, err := rand.Read(random)

    if err != nil {

        return "", err

    }

    return base32.StdEncoding.EncodeToString(random), nil

}

```

Sau khi app nhận được secret, sẽ turn thành QR code và đưa cho user. QR code này sẽ được user dùng Authenticator application để quét.

Khi đã có code, user input vào và app sẽ tiếp tục make request lên API `Generate2FA`. Nếu thành công thì user đã được enable 2FA.

Trong hàm `Generate2FA` chúng ta cần verify được code gửi lên và secret lấy từ redis có valid hay không. Sử dụng library trên như sau:

```go

otpc := &dgoogauth.OTPConfig{

      Secret:      info.Secret,

      WindowSize:  3,

      HotpCounter: 0,

}

valid, err := otpc.Authenticate(req.Code)

if err != nil || !valid {

    return errors.New("Invalid authenticate code")

}

// success validation

```

(Các bạn có thể đọc thêm source code để biết function Authenticate hoạt động như thế nào, chỉ có 1 file go thôi).

Sau khi validate xong thì mình trả về Recovery codes luôn (cho tiện ở phía application). 

Còn đây thì flow khi login, nếu cần thêm 2FA thì sẽ require code từ user, sau đó upgrade cái access token đó (simply add thêm 1 fields `2fa: true`) để nhận dạng là đã 2FA authenticated rồi.

![alt text](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/gvri4aj6di_Screen%20Shot%202018-05-25%20at%206.11.36%20PM.png)

## Kết luận

2FA là một kĩ thuật dùng để enhance security layer của một application. Hay nói cách khác, bạn khóa 1 cửa bằng 2 cái khóa luôn an toàn hơn 1 cái. Trong xã hội đầy loạn lạc này thì ứng dụng nào quan trọng, chứa nhiều sensitive data và liên quan tới tiền bạc thì cứ auto bật 2FA thôi.

## References

- https://engineering.gosquared.com/building-two-factor-authentication

- https://authy.com/what-is-2fa/
