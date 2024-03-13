---
title: "和估值大師學估值4-DCF(Discounted Cash Flow Valuation）現金流折現法公式"
date: "2021-05-03"
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

注：本文是學習估值大师Prof. Aswath Damodaran（達摩德仁教授）估值課程的學習筆記。附[視頻鏈接](https://www.youtube.com/watch?v=oi6M5KBWydg&list=PLUkh9m2BorqlJsEfix7R9jtSXClFZhGvC)和[相關資料](http://pages.stern.nyu.edu/~adamodar/)（Session3&4）

## 1.**DCF(Discounted Cash Flow Valuation）現金流折現法介紹**?

**1)**現金流折現法是指企業的內含價值是其未來可以創造的現金流按照一定的風險折現到今天的價值總和。是基於現金流、增長率、風險等基本面的估值方法。

基本上DCF分為兩部分：

**D,Discounted,折現率。**

為什麼要折現，因為資產的時間價值，也是因為任何投資都有一定的風險。

**CF,Cash Flow,現金流**。

作為價值投資者，我們買股票是投資一個事業，我們的價值應該來自於這個事業未來賺取的現金流量。

## 2.**根據現金流的風險，DCF現金流折現法分為兩類：**

在考慮公司的現金流時，是計算期望的現金流呢？還是保守一點，只計算肯定可以得到的現金流？當然，前者的風險會高一點。

為什麼要做這個區分呢？主要是二者的折現率不同。

做這個區分是為了提醒我們考慮折現率的時候要考慮我們的現金流是期望的現金流（風險相對高）還是保證的現金流（風險相對小）。

**1）Expected Cash Flow**計算公式

![](images/公式3.jpg)

E是第1年的**預期現金流**，E是第2年的**預期現金流**,......E是第n年的**預期現金流**。

r是體現風險的折現率。

**2）Guaranteed Cash Flow**計算公式

![](images/公式4.jpg)

CE是第1年的**保證現金流**，CE是第2年的**保證現金流**,......CE是第n年的**保證現金流**。

rf是無風險利率作为折现率。

## **3.為什麼巴菲特用無風險利率作為折現率？**

巴菲特選擇的就是Guaranteed Cash Flow。巴菲特的現金流是確定的現金流，而不是預期的現金流。我們對現金流越確定，越可以用接近無風險的折現率。如果計算的是預期現金流，而學巴菲特用無風險利率來折現的話，勢必會高估企業的價值。

## **4.DCF現金流計算的基本步驟：**

![](images/DCF-big-1024x525.jpg)

**1）確定折現率**

不同的現金流匹配不同的折現率。折現率可以是未考慮通貨膨脹的名目利率，也可以使實質利率。

**2）計算現金流**

通過財務報表計算出當前的現金流（FCFE,FCFF），還要考慮研發費用資本化等。

**3）預估增長率**

研究公司的主營業務增長率為多少。有了增長率，可以計算出每年的預期現金流。

**4）計算終值**

上市公司理論上可以一直活下去，但是用現金流折現法時需要假設一個終值。假設這個終點的時間，公司價值多少。

## 5.根據**現金流的利益歸屬**不同，****DCF現金流折現法的兩個模型：****

前面提到我們投資公司，為的是可以分配未來的現金流。

公司融資來源分為股權方和債權方（借貸和企業債權）。這兩方都可以分配現金流。

債權方可以從現金流中分配利息和返還本金。剩下的現金流屬於股權方。

詳情請參考[一次弄懂所有的現金流](https://fulltimemammy.com/%e4%b8%80%e6%ac%a1%e7%90%86%e8%a7%a3%e6%89%80%e6%9c%89%e7%9a%84%e7%8f%be%e9%87%91%e6%b5%81%ef%bc%9aebitdacf-ocffcffcff%e5%92%8cfcfe/)。

所以根據現金流的不同的利益歸屬，DCF現金流折現法有如下兩個模型。

## **1）Equity Valuation股票自由現金流估值**

股權方享有的現金流稱為**股票自由現金流FCFE，Free Cash Flow to Equity**，如果公司願意發股息的話，這也是公司可發放的股息的最大值。

**FCFE=營運現金流-資本支出-還債+借新債**

![](images/FCFE1.jpg)

![](images/Dcf-fcfe-1024x420.jpg)

圖片來源：http://pages.stern.nyu.edu/~adamodar/

## **2）Firm Valuation公司自由現金流估值**

股權方和債權方一起分享的現金流為**公司自由現金流FCFF，Free Cash Flow to Firm**。

![](images/FCFF-1024x119.jpg)

**FCFF=營業收入+折舊-稅金-非現金營運資金變動-資本支出。**

![](images/DCF-FCFF-1024x421.jpg)

圖片來源：http://pages.stern.nyu.edu/~adamodar/

兩個模型計算結果：

公司自由現金流估值-長期負債=股票自由現金流估值。

注：本站所有學習達摩德仁教授課程的心得文章均已獲達摩德仁教授授權。
