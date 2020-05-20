---
title: Một số papers về xử lý nội dung Manga
published: true
date: 2016-11-18 18:02:04
tags: research, opencv, hacking
description: 
image:
---
Vừa rồi mình có viết một bài về [Nhận diện khung hình trong Manga](https://thefullsnack.com/posts/manga-frame.html), nay tìm ra một vài bài báo nghiên cứu thêm nhiều thứ khác cũng trong lĩnh vực nghiên cứu này, note lại để đọc dần dần :D

[1. A Robust Panel Extraction Method for Manga](http://visal.cs.cityu.edu.hk/static/pubs/conf/mm14-panels.pdf)

Tương tự như nội dung bài viết vừa rồi của  mình, nhưng sâu hơn rất nhiều, giải quyết được nhiều trường hợp đặc biệt của các khung truyện hơn, đồng thời còn có nói về kĩ thuật lọc bỏ các nội dung nối giữa các khung truyện (ví dụ như text, sfx,...) để phân tách chính xác hơn.

[2. Manga Content Extraction Method for Automatic Mobile Comic Content Creation](https://www.academia.edu/26246675/Manga_content_extraction_method_for_automatic_mobile_comic_content_creation?auto=download)

Bài báo nói về phương pháp bóc tách các nội dung như khung truyện, khung thoại, lời thoại (tiếng Nhật) ra từ bản scan và tổng hợp lại sử dụng ComicXML để lưu thông tin, nội dung của từng khung hình. Kết quả nghiên cứu này cho độ chính xác 81.6% khi nhận diện khung hình đối với các bản scan thô (non-flat, scan không phẳng), chính xác 100% khi nhận diện các khung thoại và 93.75% khi nhận diện các kí tự tiếng Nhật.
