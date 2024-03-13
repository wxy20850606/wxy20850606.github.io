---
title: "各種資產報酬率學習，ROE,ROC/ROIC,ROA,Cash Flow ROIC(CFROI)"
date: "2021-06-11"
categories: 
  - "投資學習"
  - "會計"
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

註：本文主要參考估值大師Aswath Damodaran的論文，是從估值角度，而非會計角度。

本文主要內容：

- ROE是什麼？計算公式，怎麼使用？多少算高？
- ROC/ROIC是什麼？計算公式，怎麼使用？多少算高？
- ROE和ROC關係？
- ROA是什麼？計算公式。
- Cash Flow ROIC是什麼？計算公式？
- 會計報表裡需要調整的地方。

相關知識：

- 計算全部使用Book Value,這是整個估值和企業金融中唯一使用Book value的地方。Book value是账面价值，即财务报表上记录的价值，非市场价值。
- 公司有兩種融資方式：Equity（股權）和Debt（債務），Debt投資人可以分配利息並要求返還本金，Equity投資人理論上可以分享所有除去利息的收益。
- Equity裡包含Cash（現金），現金一般只能帶來1%的無風險收益，其收益列在Operating income下面的Other income.所以現金帶來的收入沒有貢獻到主營業務收入裡面。
- 所有公式都是基於財報，建立在財報是正確無誤的假設上。這裡我們要給個問號，因為財報裡有很多值得商榷的地方。
- 所有公式的分子用的數據都是今年的數據，分母用的都是上一年的數據。因為獲利都需要一段時間。

## 1.ROE是什麼？怎麼計算？

**1.1ROE是什麼？**

ROE, Return on Equity,股權報酬率,衡量公司利用股權即自有資本賺錢的能力。

**1.2計算公式：**

**ROE=Net Incomet/Equity(book value)** t-1

**只有ROE的分子是Net income,因为Net Income是去除了債權投資人可分配的利息收入後股權投資者可享受的收益。**

**1.3 Non-cash ROE**

因為ROE的分母裡面包含Cash，而Cash只可以賺取無風險收益的收入，所以和運營資產一起混在ROE裡會使得數據失真。

比如蘋果公司有大量的現金，現金只可以帶來1-1.5%的收入，如果想知道蘋果的主營業務的賺錢的能力的話就要用Non-cash ROE.

公平起見，可以用Non-cash ROE來判斷公司主營業務賺錢的能力。

**Non-Cash ROE=(Net Incomet\-Interest income from Cash(1-tax rate))/(Book value of Equityt-1\-Casht-1)**

什麼時候用ROE，什麼時候用Non-cash ROE,得看比較的對象Cost of Equity裡面包不包含Cash。

如果Cost of Equity裡面有放入Cash，就要ROE,如果Cost of Equity裡沒有Cash，就需要用Non-cash ROE.

（達摩德仁教授的估值模型裡Cost of equity=無風險利率+（cash adjusted）levered bottom-up beta\*股票風險溢價，需要和Non-cash ROE比較）

**1.4 ROE很高有兩個原因：**

1）公司的業務很賺錢。

像Google，FB，Apple，TSMC，這些公司的ROE都大於20%，是因為公司的業務賺錢。

花旗銀行退出台灣的消金業務而專攻財富管理，就是因為金融服務業ROE很高。


2）業務普普，但用債務灌水，導致ROE虛高。

比如公司ROE很普通，只要借錢賺的錢比借錢的成本高，借錢賺的這部分就會灌水給ROE.

債務很高的行業，要特別看ROE有無灌水。

**1.5 ROE 怎麼用？**

ROE在估值中的作用是幫助我們預估公司未來Net Income和Earning per share的成長率。

在股票投資中的作用是幫我們看公司的獲利能力，選出擁有好項目的好公司。

對於公司來說，可以和其Cost of Equity比較，在ROE大於Cost of Equity的時候投資。

**1.6 ROE多少算好？**

下面是不同產業平均ROE數值。（原始數據來源：達摩德仁教授網站[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)）

美股全市場各產業平均ROE（除去銀行股）為7.78%。

1）ROE>15%的產業。屬於很高的ROE.

![](images/image-39.png)

source:[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)

2) 10%<ROE<15% 的產業如下：

![](images/image-41.png)

urce:[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)

3）絕大部分的產業ROE都小於10%，太多就不列出了。

## **2.1ROC/ROIC 是什麼？**

**2.1 ROC是什麼？**

ROC或ROIC，資本回報率，Return on Capital 或者Return on invested Capital是一樣的。 衡量公司用已投資的資本賺錢的能力。

**2.2ROC/ROIC 計算公式：**

**ROC/ROIC=EBITt(1-tax rate)/Book value of Invested Capital**t-1

其中**Book value of Capital=Book value of Debt+ Book value of Equity-Cash；**

使用EBIT或Operating income，而非Net income；
 tax rate可以用實際稅率或者邊際稅率；
使用Book Value；
 EBIT使用今年的數值，而Book value of invested Capital應該使用1年前的數值。

  1. 為什麼不是用Net income**

因為得到Net income會減去利息。但是利息收入是分配給Debt投資人的收入，既然分母裡有Debt，分子裡當然也要有Debt的收入。

如果想用Net income,可以用如下公式轉換。

After-tax operating income = Net Income + Interest Expenses (1- tax rate) – Nonoperating income (1 – tax rate)

  2. 為什麼要用Book Value而非Market Value**

我們換一種角度看資產負債表，把資產分為：現有資產和增長資產，把負債分為：債務和股權。

那Market Value市值，是我們估計公司已有資產和未來增長資產可以帶來收入的價值總和。

但是未來增長資產目前還沒有獲利，只是一種未來展望。

所以要計算現有資產的獲利情況就只能用賬面價值。

![](images/image-46-1024x328.png)

source:[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)

  3. 為什麼分母需要減去Cash**

因為ROC是衡量主營業務賺的錢，而Cash只能賺約1%無風險收益的錢且屬於other income，為了一致要減去Cash。

對於有大量現金的公司，如果不減去Cash會導致ROC低估。

  4. Invested Capital也可以用下面兩個公式計算。**

Invested Capital = Fixed Assets + Current Assets – Current Liabilities – Cash  
\= Fixed Assets + Non-cash Working Capital

**2.3 ROC 怎麼用？**

ROC在估值中的作用是幫助我們預估公司未來Operating Income的成長率。

在股票投資中的作用是幫我們看公司運用投資資本的獲利能力，選出擁有好項目的好公司。

對於公司來說，可以和其Cost of Capital比較，在ROC大於Cost of Capital的時候投資。

**2.4 ROC多少算好？**

美股全市場產業平均After-tax ROC（除去銀行股）為10.58%。

各個產業ROC平均值（原始數據來源：達摩德仁教授網站[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)）

1）ROC大於15%

![](images/image-42.png)

source:[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)

2）ROC小於15%大於10%

![](images/image-43.png)

source:[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)

3）大部分產業ROC小於10%，太多不再列出。

## **3.ROC和ROE比較**

**3.1 ROC和ROE主要差別**

ROE的分母沒有減去Cash，而ROC的分母有減去Cash。

**3.2ROE和ROC換算公式**

**ROE=ROC+D/E(ROC-i(1-t))**

其中：ROC=EBIT(1-tax rate)/Book value of Capital
           D/E=Book value of Debt/Book value of Equity
            i(1-t)=after tax cost of Debt, t=tax rate on ordinary income
            Book value of Capital=Book value of Debt+ Book value of Equity-Cash 

**3.3 ROE灌水舉例**

當公司的ROC大於after-tax Cost of Debt時，就可以用債務來來拉高ROE.

如果公司after-tax Cost of Debt有5%，但是借钱去做一个15%的项目，賺的10%的利差就會拉高ROE.

舉例：某公司

ROC=20%

Debt/Equity Ratio=70%

After tax Cost of Debt=5%

ROE=ROC+D/E(ROC-i(1-t))=20%+0.7\*(20%-5%)=**30.5%.**

所以當看到ROE高的時候，要看下為什麼ROE高，是業務本身賺錢，還是債務灌水。

## **4.ROA是什麼？計算公式？**

**4.1 ROA是什麼？**

ROA, Return on Assets，資產報酬率，是反映公司所有資產獲利的能力。

**4.2 計算公式：**

**ROA=EBITt(1-tax rate)/Total assets** t-1

4.3 ROA沒有什麼參考價值，達摩德仁教授沒做詳細討論，此處不贅述了。

## **5.Cash Flow ROIC(CROCI)**

**5.1Cash Flow ROIC(CROCI)是什麼？**

ROC,ROE,ROA都是從會計角度來衡量獲利，而非從現金流角度。所以想要實際獲得現金流量的角度來分析公司的獲利情況就可以使用Cash Flow ROIC。

**5.2 Cash Flow ROIC(CROCI)公式：**


      
其中：Gross Fixed Assets = Net Fixed Assets + Accumulated Depreciation.

## **6.財報裡需要調整的地方。**

有如下三個影響比較大的地方需要調整。

**6.1研發費用資本化。**

研發支出明顯是為了未來而投資，是超過一個會計年度的。但是在會計報表裡都被當成了營業費用在當年扣除。

會計在計算資本支出的時候的關鍵指標是這些支出是否超過一個會計年度。用在一個會計年度內的就是營業費用，可以使用超過一個會計年度的就是資本支出。比如台積電蓋了新的工廠，可以用好多年，即是資本支出。

那研發支出明顯是為了未來而投資，是超過一個會計年度的。

所以研發支出應該是資本支出。

為什麼會計幾乎都全部把它們費用化呢？怎麼把費用化的研發支出資本化呢？

詳情請參考下面兩篇文章：

[費用化的研發支出（R&D）重新資本化](http://費用化的研發支出（R&D）重新資本化)

**6.2企業併購**（太難有待學習，需要的人請自行看教授的論文，Page40-45）

**6.3 Cross holding**（太複雜，需要的人請自行看教授的論文，page45-48）

交叉持股在財務報表裡的體現：

1）Minority passive holdings

如果持股只有幾%且對公司的營運沒有影響，要按比例在損益表上的其他收入中列出dividend，要在資產負債表上列出做這些投資的原始投資資金。

舉例：波克夏持有蘋果公司5.4%的股份，其損益表只列出從蘋果收到的股息收入，其資產負債表只列出其原始投資蘋果的資金。所以巴菲特曾在股東信裡質疑財報無法真實反映波克夏的價值。

2）Minority active holdings

大部分的少數持股都歸為Minority active holdings。比如只持有10%，但是對公司的營運有一定的影響。

舉例，持有10%的其他公司，要在損益表中列出10%的淨收益（損失），在資產負債表中列出原始的投資加或減Retained earning（留存收益）。

3）Majority active holdings

超過50%的股權需要合併報表。合併報表的意思是營收合併，淨利合併，費用合併等，再在負債中列出不屬於自己的部分。

舉例：A公司持有B公司60%的股份，在合併報表中，營收和淨利會包含100%的B公司，然後再負債中在列出不屬於自己的40%。

在計算資本報酬率的時候，可以選擇將交叉持股的部分算入，或者是拿掉交叉持股只計算母公司的部分。教授的論文裡有各種處理方式的公式，想要研究的人請移步。

## **7.總結**

![](images/image-50.png)

參考資料：

1.達摩德仁教授的論文：[Return on Capital (ROC), Return on Invested Capital (ROIC)and Return on Equity (ROE): Measurement and Implications](http://people.stern.nyu.edu/adamodar/pdfiles/papers/returnmeasures.pdf)

[2.Session 10: Growth Rates - Historical, Analyst and Fundamental](https://www.youtube.com/watch?v=xBfE6rUvCKk&list=PLUkh9m2BorqlJsEfix7R9jtSXClFZhGvC&index=12)

注：本站所有學習達摩德仁教授課程的心得文章均已獲達摩德仁教授授權。
