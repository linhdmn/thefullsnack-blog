---
title: Là framework? hay là library?
published: true
date: 2018-10-15 22:28:29
tags: random, opinion
description: Ghi chép ngắn về việc phân biệt library và framework.
image:
---
Hôm nay mình nghe bài nói chuyện giữa anh Kyle Simpson (tác giả [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)) và Jeff từ [Software Engineering Daily](https://softwareengineeringdaily.com/) [^1] <span class="mute sidenote"> :chicken: Nếu các bạn đang hỏi là: Ơ, tự nhiên lòi đâu ra cái vụ nghe podcast nữa? Thì podcast là một phong trào mới nổi trong cộng đồng developer, và tính cả thời gian đi làm lẫn đi về của mình thì một ngày tốn tầm 3, 4 tiếng lái xe, nên nghe podcast/audio book để tận dụng thời gian "chết" là một giải pháp vừa hiển nhiên vừa chỉ có lợi.</span> thì thấy một ý kiến khá là đáng quan tâm của anh Kyle về cách phân biệt giữa **library** và **framework**, note lại kẻo quên.

Đầu tiên, anh Kyle đưa ra một ví dụ về việc lái xe: Giả sử bạn lái xe, và muốn đi từ điểm A tới điểm B, mà bạn không biết đường.

- Cách đơn giản nhất, đó là mở ngăn kéo lấy ra một tấm bản đồ, nhìn vô đó để tìm đường đi, tấm bản đồ bằng giấy, và bạn phải tự tìm đường bằng cách hình dung ra con đường. Tấm bản đồ là một _công cụ_ giúp bạn tìm đường đi, nhưng **tự thân nó không đưa ra bất kì ý kiến hay chỉ dẫn nào cho bạn**.
- Ở một cấp độ khác cao hơn, chúng ta có các thể loại thiết bị kết nối GPS, **đưa ra các gợi ý và hướng dẫn** để chúng ta đi từ điểm này đến điểm khác, như Google Maps trên điện thoại. Tuy nhiên, nếu Google Maps biểu bạn rẽ trái, mà bạn lại quẹo phải, thì cũng không sao. Bạn **luôn luôn có sự lựa chọn** là nghe theo hoặc không nghe theo các ý kiến chỉ dẫn đó.
- Cấp độ cao hơn nữa, đó là [xe tự lái](https://thefullsnack.com/posts/life-with-robot.html), bạn ngồi lên xe, ra lệnh cho nó chạy từ chỗ này đến chỗ kia, và nó sẽ tự tìm đường để đưa bạn đến nơi cần đến, có nghĩa là bạn đi theo nó, và bạn **không có sự lựa chọn nào khác** <span class="mute sidenote"> :chicken: Chỉ là đại khái thôi, thực ra xe tự lái thì bạn vẫn có thể can thiệp được vào việc nó lái, và đó là luật.</span> ngoài việc ~nằm im và hưởng thụ~ ngồi yên cho nó lái.

<div class="mute">
 ■ :chicken: Đọc tới đây hẳn các bạn cũng đã hình dung ra được thế nào là framework và library rồi đúng không? đọc tiếp đi.
</div>

Ở trên là 3 cấp độ đại diện cho 3 cách phân loại: **library**, **framework** và **platform**, lần lượt dựa trên mức độ _opinionated_ của chúng.

- **Library** là các loại công cụ chỉ đơn thuần là công cụ, không đưa ra bất kỳ ý kiến hay chỉ dẫn nào để bạn phải làm theo, bạn có thể tùy ý sử dụng và tùy biến nó theo cách của bạn. jQuery, underscore, lodash là các ví dụ về library, chúng chỉ cung cấp cho chúng ta các công cụ cần thiết để tùy ý lựa chọn và sử dụng chứ ta không cần phải theo một quy tắc gì.
- **Framework** là các công cụ _opinionated_, nó đưa ra các quy tắc và hướng dẫn để chúng ta phải tuân theo, tuy nhiên nếu không theo thì cũng chả chết ai. Angular là một framework, vì nó bắt bạn code theo quy tắc của nó (từ cách đặt tên, cho tới cách tổ chức cấu trúc dự án, service, module, cách viết directives...). React thì nằm đâu đó giữa framework và library vì nó không bắt buộc chúng ta phải tuân theo một quy cách nào, mỗi người có một cách sử dụng React riêng, nhưng vẫn phải tuân theo một số quy tắc mà nó đưa ra (ví dụ các lifecycle handler của một component, và bạn có thể tùy ý setState hoặc xài Redux,...).
- **Platform** là các công cụ _cực kì opinionated_, <span class="mute sidenote"> :bulb: Cũng có người định nghĩa platform theo nghĩa rộng hơn, là bao gồm cả hệ điều hành, phần cứng, nơi mà các ứng dụng, framework chạy. Tuy nhiên cách định nghĩa này có vẻ không liên quan đến platform trong bài viết.</span>bạn phải tuân theo hoàn toàn mọi quy tắc mà nó đưa ra, làm khác đi thì không được. Ví dụ Wordpress nằm ở giữa khoản framework và platform, một bộ theme phải có các thành phần này kia, phải có `function.php` (phải ko ta, lâu rồi không code PHP), .NET Framework là một platform. PhoneGap là một platform.

**Notes**
[^1]: [Podcast: JavaScript Concurrency with Kyle Simpson](https://softwareengineeringdaily.com/2016/06/12/2610/)
