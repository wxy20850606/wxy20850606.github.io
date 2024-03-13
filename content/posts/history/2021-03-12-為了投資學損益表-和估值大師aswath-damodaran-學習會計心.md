---
title: "為了投資學損益表——和估值大師Aswath Damodaran 學習會計心得筆記二"
date: "2021-03-12"
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

本文是和估值大師Aswath Damodaran 學習損益表的心得筆記之二，附學習[視頻鏈接](https://www.youtube.com/watch?v=Q8wKr1QDSwg&list=PLUkh9m2BorqmKaLrNBjKtFDhpdFdi8f7C&index=4)和[講義習題鏈接。](http://people.stern.nyu.edu/adamodar/New_Home_Page/webcastacctg.htm)

註：Aswath Damodaran是估值權威，而非會計師，他開課目標是教授學生學習夠用的會計來幫助估值，非專業會計。**Aswath Damodaran達摩德仁的youtube頻道和網站就像個知識寶庫，強烈推薦想要學習投資或者估值的人觀看。**

## 1.Income statement損益表是什麼？

損益表是三大財務報表之一，目的是介紹在一個會計年度內公司賺了多少錢。各種收入多少，各種成本有多少，最後收入減去成本算出賺了多少錢。

想一想如果把自己每年的收入和支出都列出來的話，收入減支出就是淨收益，就製作了自己的年度損益表。

![](images/个人损益表-1.jpg)

公司的損益表也是如此，列出各種收入和各種費用，得到淨利潤。為了更好的衡量企業獲利的情況，除了淨利潤之外，還有毛利潤、營業利潤和稅前利潤等指標。

![](images/损益表.jpg)

## 2.企業各種收入介紹

1）營業收入

主營業務收入，比如蘋果公司，主營業務收入就是賣手機、平板、耳機等和提供服務賺取的收入。

2）營業外收入

a. Cash and marketable securities 現金及短期有價證券。

很多公司有大量的現金，這些現金都是投資在短期有價證券比如短期國庫券、商業票據等流動性高幾乎沒有風險的類現金資產。其帶來的收入就會顯示在其他收入裡。

b. Cross holdings in other companies 其他公司持股。

上市公司持有其他上市公司的股票。比如巴菲特的公司就大量持有其他上市公司的股票，波克夏持有蘋果總共5.4%的股票。

![](images/巴菲特-300x158.jpg)

數據來源：[巴菲特2021致股東信](https://www.berkshirehathaway.com/letters/2020ltr.pdf)。

如果持股小於50%，要按比例在損益表上列出淨收益/淨損失。比如波克夏的年報就需要列出蘋果公司5.4%的淨收益。如果持股大於50%，就需要整合整個公司的收入支出等，再在資產負債表上列出不是屬於自己持有的部分。

## 3.企業各種費用支出介紹。

1）Costs of Goods sold，銷貨成本，簡稱COGS

與商品生產販售直接相關，與營業費用相對應。

一般在損益表上直接列在營業收入Revenues的下面一行，好計算毛利潤。

營業收入-銷貨成本=毛利潤。

2）Operating Expenses，營業費用，簡稱OPEX

營業費用，是企業需要維持運營需要支付的持續性的支出，比如行政費、房租費、廣告費等。和生產成本相對應，營業費用沒法和生產直接相關。

在損益表上銷貨成本下一行。

有些公司的報表上直接用Selling, General & Administrative Expense (SG&A)來表示營業費用。

營業收入-銷貨成本-營業費用=營業利潤。

3）Financing Expense，財務費用

主要指利息，在損益表上面營業費用下一行，有些公司就沒有這個費用。

營業收入-銷貨成本-營業費用-財務費用=稅前利潤。

4）Capital Expense,資本支出，簡稱CAPEX

和營業費用想對應。營業費用是指本會計年的內的費用。而資本支出是指為了未來（超過1年）的收益而做的投資，包含新增廠房或者維護升級廠房所產生的固定資產，或者購買專利等無形資產。

因為不是本年度的費用，而是給未來所用，所以按照權責發生制，不會全部直接以費用列入損益表中，而會以Amortization & Depreciation（攤銷和折舊）的形式出現在每年度的損益表上。

也會在資產負債表中體現出來，比如固定資產增加的部分就很有可能是資本支出。

## 4.Deferred Revenue 遞延收入

根據會計的權責發生制，交易發生的時候就編列入賬。那比如軟件公司，都流行用折扣價吸引客戶成交了超過1年的合約。比如3年的合約，這樣的收入要怎麼算呢？

如果把這3年的收入都算在一年，有些不公平，因為要提供3年的服務。

目前規定是把屬於今年的部分提取出來，當做今年的收入， 剩下的算作Deferred Revenue 遞延收入，也可以叫做Unearned revenue。

請看下面Salesforce的現金流量表，Unearned revenue竟然有4684百萬美金。在做估值的時候顯然要好好考慮這個收入。

![](images/salesforce-1024x568.jpg)

## 5.不同的折舊思維

折舊通常出現在營業費用中，為了把資本支出分攤到一定的年限裡當做費用。主要有下列三種折舊思維。

1）Economic depreciation經濟折舊思維

根據資產的效用折舊，越老舊，折舊越多。

2）Accounting depreciation會計折舊思維

比如直線折舊法，比如一項固定資產以直線折舊法10年內折舊完畢，那就是一年折10%，到第十年末殘值為0.。

會計有一套自己的方法來處理折舊，這些方法只是方便會計操作，比較機械化，有時並非完完全全與現實一致。

3）Tax depreciation稅務折舊思維

單純為了最大避稅考量來編列折舊多少。

## 6.extraordinary income/expenses特別收入/支出

一次性的支出或者收入。比如一次性的出售某資產，或者一次性的法律費用支出。會偶爾出現，不會每年出現在損益表上。

注：本站所有學習達摩德仁教授課程的心得文章均已獲達摩德仁教授授權。
