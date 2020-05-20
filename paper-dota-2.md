---
title: Một vài papers về DotA 2 và Machine Learning
published: true
date: 2016-10-02 10:26:17
tags: research, hacking
description: Paper đầu tiên về việc dùng số liệu trong DotA 2 vào một vấn đề machine learning, của tác giả Kevin, mục tiêu của paper là sử dụng Machine Learning để recommend counter pick trong DotA dựa trên tương quan hero 2 phe dire và radiant, lịch sử thắng thua,...
image:
---
## How Does He Saw Me?
Link bài báo: [http://cs229.stanford.edu/proj2013/PerryConley-HowDoesHeSawMeARecommendationEngineForPickingHeroesInDota2.pdf](http://cs229.stanford.edu/proj2013/PerryConley-HowDoesHeSawMeARecommendationEngineForPickingHeroesInDota2.pdf)

Paper đầu tiên về việc dùng số liệu trong DotA 2 vào một vấn đề machine learning, của tác giả Kevin, mục tiêu của paper là sử dụng Machine Learning để recommend counter pick trong DotA dựa trên tương quan hero 2 phe dire và radiant, lịch sử thắng thua,...

## To Win or Not To Win
Link bài báo: [http://cseweb.ucsd.edu/~jmcauley/cse255/reports/wi15/Kaushik_Kalyanaraman.pdf](http://cseweb.ucsd.edu/~jmcauley/cse255/reports/wi15/Kaushik_Kalyanaraman.pdf)

Dự đoán thắng thua dựa trên team pick, dùng chung feature vector với paper ở trên

## DotA 2 Win Prediction
Link bài báo: [http://cseweb.ucsd.edu/~jmcauley/cse255/reports/fa15/018.pdf](http://cseweb.ucsd.edu/~jmcauley/cse255/reports/fa15/018.pdf)

Dự đoán thắng thua bằng 2 cách, một cách là dựa trên dữ liệu cuối trận đấu như là Gold-per-minute, Exp-per-minute, Kills-per-minutes, và một cách là dựa trên team pick

## Predicting the winning side of DotA 2
Link bài báo: [http://cs229.stanford.edu/proj2015/249_report.pdf](http://cs229.stanford.edu/proj2015/249_report.pdf)

Vẫn là dự đoán thắng thua dựa trên team pick, lấy cơ sở từ bài paper của Kevin ở đầu kia, sử dụng logistic regression và dùng chung 1 bộ data, dùng chung feature vector

## Result Prediction by Mining Replays in DotA 2
Link bài báo: [https://www.diva-portal.org/smash/get/diva2:829556/FULLTEXT01.pdf](https://www.diva-portal.org/smash/get/diva2:829556/FULLTEXT01.pdf)

Phân tích replay để dự đoán kết quả trận đấu, có giới thiệu qua nhiều thuật toán nhưng cuối cùng kết luận Random Forest là cho kết quả chính xác nhất, bài này nói khá kĩ ở khúc phân tích đặc thù dữ liệu của một file replay DotA 2
