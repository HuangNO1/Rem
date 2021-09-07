---
title: "2021 年 7 月字節跳動安卓崗位 - 提前批面試經驗（已涼）"
date: 2021-09-07T18:17:28+08:00
# 副標題
subtitle: ""
# 上次修改的時間
lastmod: 2021-09-07T18:17:26+08:00
draft: false
description: ""
license: ""
tags: ["ByteDance", "互聯網", "大廠", "Android", "2021", "秋招提前批", "實習", "面試", "LeetCode", "CS"]
categories: ["Moment"]
featuredImage: "/featuredImage/compressed/an_interview_experience_about_bytedance_android_2021_07.jpg"
---

## 前言

現在已經度過了一整個暑假，我也從大三變成了大四生，已經九月份了，感慨三四年這樣就過去了，在這暑假期間我已也投過一家南京小公司實習（透過本專業的教授），但最後考慮到薪資問題就拒絕了後續（主要是薪資談不攏），也在七月份度過了兩到三週中興通訊的在校合作實訓，反正能**拿到 4 學分的實習學分**，不過實訓內容說實在一言難盡，我也不在這篇文章提出。

這篇文章主要是針對上學期大三下的時候我因為選了安卓應用開發的選修課，字節跳動的員工內推我，讓我可以面試。

> 我在這裡還是想吐嘈一下，**不管哪個公司說什麼有實習機會都不要太相信**，因為大多數都只是幫你內推而已，中興通訊的實訓原本也是說有實習名額，HR 也說可以是直接內定的 Offer 名額，然而也只是內推，包括這篇文章提到的字節跳動也是一樣，一開始說有實習機會，最後所有同學都去要了簡歷給內推。

我荒廢了這個暑假後，才開始想寫這篇文章重新整理一下心態，也因為太久沒有更新 Blog 文章，所以有些過意不去。

## 面試前準備

在內推面試時，已經是學期末要**期末考**的時候了，說實在，時間很緊，我要同時準備期末考和面試，所以我將技術面一面跟字節跳動 HR 溝通在期末考結束後一週，**在一週的時間內閱讀大量的面經還有之前自己整理的八股文內容，還有自己找到的一些八股文內容**，由於內推人有說字節跳動的安卓崗**可以不會安卓，接受初學者，但是必須至少要精通一門語言**，然後如果能精通 Java 當然是更好，所以我沒有特別準備安卓相關的問題。

## 技術面面經

**字節跳動 - 技術中台 - 安卓客戶端實習生**

**業務一面(50 分鐘)，牛課網平台面試**

1. 簡單自我介紹一下

xxxxxx

2. 根據我的安卓項目問關於安卓應用開發課程是怎麼跟字節跳動客戶端合作的，學了多久安卓

老師與字節客戶端的人員進行合作，先是有理論課，後面有實踐課，給了一個大作業，完成業務需求...blabla

學了一個月安卓

3. 你在安卓項目中做了哪些構想

像是頁面的構思，歡迎界面與主界面 主界面有不同的頻道，像是推薦、數碼。每個頻道有自己的....blabla

還做了點擴展...blabla

4. 你這個項目中有哪些難點，怎麼實現的 可以說明一下嗎

像是一個列表中要展示三種不同的卡片...blabla...dataModel recyclerview 

5. 請問你是 java 比較熟還是 C++ 比就熟

C++ 比較熟悉，但是 Java 有一定的基礎

面試官：那我問你一些 java 的問題

6. 請問 java 的線程狀態你能說一下嗎

(fuxk you 怎麼一開始就這麼難)java的線程有像是堵塞 同步之類的狀態，線程同步可以使用 排斥鎖 自旋鎖 讀寫鎖，排斥鎖像是 synchronized，堵塞的話就是資源被一個線程被鎖住 下一個線程無法使用資源，等鎖釋放才能讓下一個線程進行操作...blabla

7. 請問除了這些，還有哪些狀態

...我不了解（想到自己的完了 接下來肯定深問 java 線程 鎖）

8. 能說一下 jvm 中是怎麼判斷是可回收對象

使用引用計數器與可答性的計算。當一個對象被引用的時候計數器會加一，可達性算法就是有個 root 如果不能達到就判定不可回收

9. 請說說一下可達性

有一個鍊...blabla（不知道自己在說什麼）

10. 那說一下有哪些東西可以作為這個 root

...我不了解(破防了)

11. 你前面提到了volatile 能說一下嗎

volatile 是存在內存中然後線程是可見的 也具有可序性....blabla

12. 能說一下 Synchronized 修飾不同地方的區別嗎

類鎖 對象鎖 變量鎖...blabla

13. 能具體說一下區別嗎

...我不了解

14. 寫一個線程安全的單例模式

寫了懶漢模式，synchronized 修飾在 static methods

15. 能做一些優化嗎

我：...???能做什麼優化

面試官：像是將 synchronized 修飾到方法裡面

我：好的...

16. 你設計模式中除了單例模式之外，還有了解過其他設計模式嗎，能說一下代理模式嗎？

（完了，我沒複習代理模式）我雖然不了解代理模式但是我可以講一下適配器模式 blabla...

17. 請問你有了解過內存洩漏嗎

內部類持有外部類的引用 造成...blabla

18. 能說一下為什麼內部類持有外部類的引用，為什麼會被判定不可回收

...不知道怎麼回答，瞎扯，又舉了 android 中 handler 源碼的例子

19. 能說一下 HTTPS 是怎麼請求的嗎

使用了 SSL/ TLS 然後一開始 CA 證書認證 非對稱加密，接著傳內容...blabla

20. 我的意思是完整的一次

從應用層到物理層...還有三次握手...DNS 都說了，只是講的當下有點混亂，缺乏順序邏輯

21. 能說一下 TCP UDP 區別嗎

...blabla

22. TCP 怎麼實現可靠性，能說一下滑動窗口嗎？滑動窗口好處

...blabla

23. 能說一下 C++ 中的 static 嗎 能說一下 final 嗎

（Fuxk? final 不是 java 的東西嗎，面試結束後查到原來 C++ 也有這個 Keyword，主要用在類聲明的部份）....blabla

24. 知道 C++ 模板嗎？用過嗎？

....blabla

25. 有了解過安卓中的 MVC、MVP 等之類的布局架構嗎？他們又各有哪些優缺點或區別？

是的，我有了解過，有 MVC、MVP、MVVM，三種架構，其中...blabla

26. 請問有用過線程池嗎？能說一下如果是你 你會怎麼設計XXX....像是堵塞....

(FUXKFUXKFUXK) 沒有，後面瞎扯...舉了 wait() nofity()。

27. 我們來寫算法吧，就快速排序

我：(心中暗爽)好的...鍵盤 papapa。

面試官：能說一下是怎樣嗎

我：....blabla

28. 反問

你們部門具體有哪些業務？
你們主要的技術棧方面？平常是用 java 還是 kotlin？

結束..........
---------------------分割線-----------------

緊張，一開始完全腦袋空白。

## 結語

我面試完後，我覺得能過得機會大概一半一半，少許問到了安卓的問題，然而最後讓我寫一個快速排序，感覺像是想讓我早點結束一樣，也猜不出為什麼，面試官是女性，我面試一開始腦袋忽然覺得空白，直到慢慢要我回答一些問題才慢慢好轉。過了不到一週，我收到了拒信，感覺很遺憾，我覺得這一周時間已經要到我的記憶力巔峰時期了，因為自從高中畢業後，我就覺得我的記憶力一直在衰退下降，很多東西背完又忘記了，希望這個面經能幫助到大家，也讓大家能找到明確的方向。

最後的最後，我宣佈我的大廠夢徹底破滅，我再也不投大廠了。