---
title: "一次理解所有的現金流：EBITDA,CF/OCF,FCF,FCFF和FCFE"
date: "2021-04-16"
categories: 
  - "估值"
  - "投資學習"
tags: 
  - 投資學習
weight:
slug: ""
draft: false # 是否为草稿
comments: true # 本页面是否显示评论
reward: true # 打赏
mermaid: true #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
    image: "" #图片路径例如：posts/tech/123/123.png
    zoom: # 图片大小，例如填写 50% 表示原图像的一半大小
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

![](images/现金流-1024x564.jpg)

最近在學估值課程，常常聽到的現金流就有EBITDA,CF/OCF,FCF,FCFF和FCFE，真的是讓人很頭大，所以想寫篇文章幫助自己梳理一下。經過梳理過的知識相對不容易忘記。

先介紹下相關知識：

**1.估值的兩種方式：**

1）相對價值估值法：

通過觀察類似資產的市價來估計股票資產。最廣為所知的就是PE(市盈率)了，比如台積電PE為30，英特爾PE為10，表示英特爾比較便宜，值得買入。

很多CFA分析師喜歡用EV/EBITDA（ 企業價值倍數）取代PE來估值。

2）內在價值估值法：

內在價值，即intransic value怎麼估計呢？

公司的內在價值是通過DCF現金流量折現的方法估算。

需要四個基本數據：現有資產的現金流量（已扣除再投資需求和稅金）；現金流量預期成長率；資產融資成本；公司在預測期結束時的估值。

所以就需要用到現有資產的現金流量。評估現有資產的現金流量又可以分為兩種：一種是評估整間公司，一種是評估這間公司的股權。

FCFF,評估整間公司，就包含了股權和債權。

FCFE評估這間公司的股權，就只包含股權。

**2.公司有兩種融資方式：**

1）Equity Investor股權投資人，持有股權，可分配去除利息、債務、稅金和再投資需求後的現金流量。

2）Debt investor債權投資人，持有債權，可從公司的現金流量裡要求分配利息、償還本金。

**3.公司再投資：**

公司的再投資包含長期資產和短期資產的再投資。再投資金額除以稅後營業收入就是公司的再投資率。

- 長期資產的投資=資本支出-折舊

公司對長期資產的投資，是資本支出減去折舊後的淨資本支出。為什麼要減去折舊呢，因為折舊是過去投資折舊在今年的部分，只是會計支出，不是現金支出（可以理解為過去投資返還的錢）

- 短期資產（存貨、應收賬款、應付賬款）的投資=公司非現金營運資金的變化。

**4.折舊攤銷：**

折舊和攤銷是會計費用，而非現金支出。在損益表中折舊和攤銷屬於營業費用，予以扣除。但是這筆費用只是會計費用，實際情況是在資本支出的當下早就付出現金了。在現金流量表中需要把這筆非現金費用重新加回到營業現金流中。

**5.Working Capital營運資金：**

營運資金=流動資產（現金、存貨、應收賬款等）-流動負債（應付賬款、其他短期債務）。

6.**Non-Cash Working Capital非現金類運營資金的變動**：

非現金營運資金=非現金流動資產-非現金流動負債。還有另外一種簡化的計算方式：**非現金營運資金變動=存貨+應收賬款-應付賬款**。

下面進入正題，介紹下各種現金流。

## **1.EBITDA:**

- 定義：EBITDA是Earnings Before Interest, Taxes, Depreciation and Amortization的縮寫。主要衡量企業的主營業務產生現金流的能力。

- 計算方法：

![](images/EBITDA.jpg)

- 使用:主要用於相對價值估值法。

![](images/CF.jpg)

**EV/EBITDA 企業價值倍數。**

EV＝市值＋總負債－總現金。

功能相當於PE，在相對價值估值法中比較公司是低估還是高估。比如**EV/EBITDA 企業價值倍數**\=10，和其他同類型公司**EV/EBITDA 企業價值倍數**\=15相比，算是很便宜。

**NET DEBT/EBITDA，淨債務對息稅攤銷前利潤**。

Net Debt=總債務-現金和現金等值。

描述企業會不會因為債務過高導致現金流斷掉而破產。比如**NET DEBT/EBITDA**\=3，表示公司營運狀況在未來保持不變（淨債務和EBITDA都不變）的情況下，其主營業務帶來的現金流可以在**3年**償還完全部的淨債務。如果超過3年的話，表示公司的債務有些偏多，需要留意。

## **2.CF/OCF Cash Flow, Operating Cash Flow營運現金流**

現金流量表三大組成部分：營運現金流，投資現金流，融資現金流。請參考：[零基礎學現金流量表](https://fulltimemammy.com/%e9%9b%b6%e5%9f%ba%e7%a4%8e%e5%ad%b8%e7%8f%be%e9%87%91%e6%b5%81%e9%87%8f%e8%a1%a8-%e5%92%8c%e4%bc%b0%e5%80%bc%e5%a4%a7%e5%b8%abaswath-damodaran-%e5%ad%b8%e7%bf%92%e6%9c%83%e8%a8%88/)。

1）定義：企業主營業務賺錢的現金流量。

2）計算:

![](images/CF-1.jpg)

非現金費用 Non-cash item：比如最常見的即是Stock-based compensation。不是現金支出，現金流量表的處理方式是把這筆非現金費用加回到現金流量中。（但是估值大師達摩德仁教授卻認為不應該加回來，詳情請看[Stock-based compensation，股票薪酬要不要](https://fulltimemammy.com/stock-based-compensation%ef%bc%8c%e8%82%a1%e7%a5%a8%e8%96%aa%e9%85%ac%e8%a6%81%e4%b8%8d%e8%a6%81%e5%8a%a0%e5%9b%9e%e5%88%b0%e7%8f%be%e9%87%91%e6%b5%81%e9%87%8f%e8%a3%a1%e9%9d%a2%ef%bc%9f/)[加回到現金流量中](https://fulltimemammy.com/stock-based-compensation%ef%bc%8c%e8%82%a1%e7%a5%a8%e8%96%aa%e9%85%ac%e8%a6%81%e4%b8%8d%e8%a6%81%e5%8a%a0%e5%9b%9e%e5%88%b0%e7%8f%be%e9%87%91%e6%b5%81%e9%87%8f%e8%a3%a1%e9%9d%a2%ef%bc%9f/)。

3）使用：

營運現金流是現金流量表最重要的組成部分。

損益表的原則是權責發生制（交易成立時入賬），而現金流量表的原則是收付實現制（收到現金時入賬）。所以加上非現金類營運資金的變動可以把權責發生制的損益表的漏洞堵起來。

## **3.FCF自由現金流**

1）定義：營運現金流減去維持企業運行的資本支出後，企業可以自由運用的現金。

2）計算：

![](images/FCF.jpg)

詳細一點就是：

![](images/FCF1-1024x101.jpg)

3）使用：

FCF自由現金流又延伸出FCF**E**(Free Cash Flow to **Equity**)和FCF**C**（Free Cash Flow to **Firm**）

前面提到公司的資本結構來源於Equity和Debt，FCFE和FCFC就是把自由現金流區分開來。

## **4.股票自由現金流FCFE，Free Cash Flow to Equity**

  1. 定義：指的是淨利潤（已扣除稅金和**債務利息**）去除再投資需求（短期和長期）、負債現金流後，所剩餘的現金。

在[零基礎學現金流量表](https://fulltimemammy.com/%e9%9b%b6%e5%9f%ba%e7%a4%8e%e5%ad%b8%e7%8f%be%e9%87%91%e6%b5%81%e9%87%8f%e8%a1%a8-%e5%92%8c%e4%bc%b0%e5%80%bc%e5%a4%a7%e5%b8%abaswath-damodaran-%e5%ad%b8%e7%bf%92%e6%9c%83%e8%a8%88/)裡提到：FCFE=營運現金流加或減營運類的投資現金流，再加或減Debt類融資現金流。

![](images/dividend.jpg)

簡化一下就是：FCFE=淨利潤+折舊-非現金營運資金的變動-資本支出-還債+借新債

更進一步簡化如下。

  2. 計算方法：

![](images/FCFE1.jpg)

  3. 使用：

用於內在價值估值法也就是DCF估值。

## **5.公司自由現金流FCFF，Free Cash Flow to Firm**

  1.定義：公司自由現金流是指營業收入滿足了所有再投資需求（短期和長期）和繳納稅金後、**未支付債務利息**和本金之前所剩餘的現金。

公司融資分為股權和借債。FCFF 的Firm就包含股權人和債權人一起創造的現金流。而債權人的對公司的權利即是利息。所以FCFF沒有扣除利息。

  2. 計算：

FCFF=營業收入-稅金-再投資（短期和長期）

再投資=短期資產投資（非現金營運資金變動）+長期資產投資淨值（資本支出-折舊）

FCFF=營業收入-稅金-（非現金營運資金變動+資本支出-折舊）=營業收入+折舊-稅金-非現金營運資金變動-資本支出。

![](images/FCFF-1024x119.jpg)

  3. 使用：

也用於內在價值估值法也就是DCF估值。

## **6.FCFE和FCFF 比較：**

相同點：

都是稅後和再投資需求滿足後的現金流。

不同點：

FCFE Free Cash Flow to Equity，是從淨利潤開始計算的，扣除要付給債權人利息和負債類融資現金流（還舊債借新債），因為淨利潤是股權投資人（equity investor）的收益，是從股權投資人角度看的。

FCFF Free Cash Flow to Firm，是從營業收入開始計算，沒有扣除利息和債務的增減，因為營業收入是股權人和債權人共同擁有的收益，是從整個公司角度看的。

## **7.總比較：**

![](images/比较.jpg)

**8.其他：**

巴菲特的DCF則更為保守，不是用FCFF，也不是用FCFE，而是用事業主盈餘（owner‘s earning）則，是FCFE before debt，即不將債務產生的現金流納入計算。

![](images/FCFE-B.jpg)

## 9.本文其他參考鏈接：

[The Ultimate Cash Flow Guide (EBITDA, CF, FCF, FCFE, FCFF)](https://www.youtube.com/watch?v=sTYjXC4Udrc)

## **總結：**

當聽到現金流時，在不同的情境下有不同的現金流，所以要分清楚到底具體是哪個現金流。

