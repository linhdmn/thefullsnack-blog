---
title: Quy tắc của Hướng đạo sinh và nghề Lập trình
published: true
date: 2017-06-27 14:44:22
tags: opinion, random, reading
description: 
image:
---
Thi thoảng mình rất muốn share một vài đoạn trích hay bài viết mà mình cảm thấy hay, tuy nhiên đăng lại một đoạn trích bằng tiếng Anh hoàn toàn thì không phải ý hay vì mình biết sẽ chẳng ai chịu đọc :)) dịch ra tiếng Việt thì lại là một việc hoàn toàn cấm kị.

Cho nên bắt đầu từ bài này, mình sẽ giới thiệu một feature mới của blog đó là **Quơ-Qaise** (bắt chước [Word Wise](http://www.pcworld.com/article/2847882/family-friendly-kindle-updates-include-family-library-sharing-word-wise-definitions.html) của Amazon Kindle =))), để chú thích nghĩa của những từ ít gặp, hoặc bình luận nhố nhăng.

Đoạn trích sau là từ bài viết [The Boy Scout Rule](http://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule) của cuốn sách **97 Things Every Programmer Should Know**, đăng tải online với giấy phép [Creative Commons Attribution 3.0](http://creativecommons.org/licenses/by/3.0/us/).

---

<p></p>

<p style="text-align: right; padding: 0 35px;">
  Quơ-Qaise
  <button id="ww_on" class="btn-toggle active" onclick="toggle_wordwise(true)">ON</button>
  <button id="ww_off" class="btn-toggle" onclick="toggle_wordwise(false)">OFF</button>
</p>

<book>
  <p><word>The Boy Scouts<wise>Các Hướng Đạo sinh</wise></word> have a rule: <i>"Always leave the <word>campground<wise>nơi cắm trại</wise></word> cleaner than you found it."</i> If you find a mess on the ground, you clean it up regardless of who might have made the mess. You <word>intentionally<wise>một cách chủ ý</wise></word> improve the environment for the next group of campers. Actually the original form of that rule, written by <i>Robert Stephenson Smyth Baden-Powell</i>, the father of <word>scouting<wise>hướng đạo</wise></word>, was <word><i>"Try and leave this world a little better than you found it."</i><wise>Làm cho thế giới trở nên tốt hơn lúc mà bạn đến với nó</wise></word></p>
  <p>What if we followed a similar rule in our code: <i>"Always check a module in cleaner than when you checked it out."</i> No matter who the original author was, what if we always made some effort, no matter how small, to improve the module. What would be the result?</p>
  <p>I think if we all followed that simple rule, we'd see the end of <word>the relentless deterioration<wise>tình trạng "bê tha" hóa =))</wise></word> of our software systems. Instead, our systems would <word>gradually<wise>dần dần</wise></word> get better and better as they <word>evolved<wise>scale up</wise></word>. We'd also see teams caring for the system as a whole, rather than just <word>individuals<wise>một cá nhân</wise></word> caring for their own small little part.</p>
  <p><word>I don't think this rule is too much to ask<wise>đúng rồi, thế này có gì là quá đáng đâu</wise></word>. You don't have to make every module perfect before you <word>check it in<wise>push code</wise></word>. You simply have to make it a little bit better than when you <word>checked it out<wise>pull code</wise></word>. Of course, this means that <word>any code you add to a module must be clean<wise>code của mình phải sạch</wise></word>. It also means that you <word>clean up at least one other thing<wise>rồi mới làm sạch thằng khác</wise></word> before you check the module back in. You might simply improve <word>the name of one variable<wise>LOL...</wise></word>, or split one long function into two smaller functions. You might break a <word>circular dependency<wise>a.child = b; b.child = a;</wise></word>, or add an interface to <word>decouple policy from detail<wise>Đọc <a href="http://kristopherwilson.com/2013/11/27/decoupling-the-framework/" target="__blank">bài này</a></wise></word>.</p>
  <p><word>Frankly<wise>thực ra</wise></word>, this just sounds like <word>common decency<wise>thói quen chung</wise></word> to me — like washing your hands after you use the restroom, or putting your trash in the bin instead of <word>dropping it on the floor<wise>lol, bẩn thế</wise></word>. Indeed the act of leaving a mess in the code should be as <word>socially unacceptable<wise>xã hội không chấp nhận</wise></word> as <word>littering<wise>đốt nhà</wise></word>. It should be something that just isn't done.</p>
  <p>But it's more than that. Caring for our own code is one thing. Caring for the team's code is quite another. <word>Teams help each other, and clean up after each other<wise>gọi là đùm bọc lẫn nhau nhé =))</wise></word>. They follow the Boy Scout rule because it's good for everyone, not just good for themselves.</p>
  <right>by <a href="http://blog.cleancoder.com/">Uncle Bob</a></right>
</book>

Ngày xưa Lục Vân Tiên gặp chuyện bất bình giữa đường thì không thể làm ngơ, kết quả là thu phục được mỹ nhân (oops, nghe giống pokemon, sorry :D) tiếng thơm lưu lại muôn đời (dù là chỉ là nhân vật hư cấu). Ngày nay, chẳng ai muốn làm Lục Vân Tiên giữa đời thường, vì sợ tai bay vạ gió, chuốc họa vô thân, từ đó mà xã hội càng suy đồi. Nhưng nếu làm Lục Vân Tiên giữa rừng bad code thì cũng chả ai có thể hãm hại gì được bạn cả, nên cứ gọi là để cứu rỗi chút lương tâm còn xót lại.

<p class="special">Làm chút việc tốt, dù là không nhiều nhặn gì, để làm cho thế giới trở nên tốt hơn một chút, cũng là việc nên làm. <word>Mô phật<wise>boong...</wise></word>.</p>
