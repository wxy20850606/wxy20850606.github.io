---
title: "零基礎學資產負債表——和估值大師Aswath Damodaran 學習會計心得筆記四"
date: "2021-03-16"
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

本文是和估值大師Aswath Damodaran 學習資產負債表的心得筆記之四，附學習[視頻鏈接](https://www.youtube.com/watch?v=cSuc2HHQpxc&list=PLUkh9m2BorqmKaLrNBjKtFDhpdFdi8f7C&index=6)和[講義習題鏈接。](http://people.stern.nyu.edu/adamodar/New_Home_Page/webcastacctg.htm)

註：Aswath Damodaran是估值權威，而非會計師，他開課目標是教授學生獲得估值夠用的會計就好，非專業會計。**Aswath Damodaran達摩德仁的youtube頻道和網站就像個知識寶庫，強烈推薦想要學習投資或者估值的人觀看。**

## **1.資產負債表是什麼？**

**The balance sheet 資產負債表，**介紹在某個時間點，公司有多少錢（資產），欠了多少錢（負債）。

The balance sheet关键在balance。balance即是平衡的意思，資產=負債+股東權益，就像天平的两端，资产始终等于负债加股东权益。

![](images/balance.jpg)

1）Fixed assets 固定資產，長期使用可以帶來收益的有形實體資產，比如廠房、設備。

2）Current assets流動資產，一年以內，比如應收賬款、存貨、短期證券等。

3）Financial Assets金融資產，公司做其他投資的資產，或者持有其他上市公司的股權資產等。

4）Intangible Assets 無形資產，比如專利、商譽等。

5）Current Liabilities流動負債 ，一年以內短期負債，比如應付賬款、短期負債等等

6）Debt長期負債，大於一年的負債，比如銀行貸款和公司債。

7）Other Liabilities其他長期負債，公司其他的需要付錢的義務，比如養老金。

8）Equity股東權益是一個調劑變量，用來解釋資產和負債的差額。

## **2.Fixed and current assets固定資產和流動資產**

兩種計價模式：

1）歷史成本計價，按照資產的成本價再減去折舊來計算資產的價值，比較舊式。此計價方法只有折舊，沒有升值。

2）公允價值計價（fair value accounting），按照市場價來計算資產的價值，比較新式。

目前還沒有統一的規定要用哪一種，所以比較有年紀的公司有可能就是一直沿用歷史成本計價，而比較年輕的公司有可能採用公允價值計價。在不同計價模式下，年紀越大的資產受影響差異比較大，固定資產（比如土地廠房）又比流動資產（比如存貨）差異大。

## **3.Financial Assets金融資產**

公司的金融資產可以是持有上市公司的股票或者是持有上市、私有公司的部分股權。

1）持有上市公司的股票，按照市場價格計算。

2）持有上市、私有公司的部分股權。

- minority stake 非控股持股，股權小於50%。這其中又按照持股的原因分為兩類。

一類是交易性持股。比如巴菲特持股了很多如下很多公司，持股比例都不到50%，這些持股是交易持股。

![](images/bafet11.jpg)

所以波克夏的年報從2018年開始按照新的會計準則，需要列出這些持股的獲利或是損失。可以看到2018年市場下跌，波克夏的Investment gains是負22155M$，2019年市場上漲，波克夏的Investment gains是正的71123M$。

![](images/brk22.jpg)

另一類是持股不是為了交易，而是做為戰略投資，像是可口可樂對各個國家的瓶裝廠的投資應該就是戰略投資。這一類的投資會按照成本價入賬，然後按照Book value（賬面價值）加入到retained earning保留盈餘。

- majority stake 控股持股，股權大於50%，需要合併報表。

比如持股60%，就需要假裝100%都是自己的，算入100%的資產，然後再把不是自己的40%算為負債，列為minority interest。

## **4.Intangible Assets 無形資產**

我們一般人一想到無形資產一般都會想到專利、品牌商標、著作權等等，但是會計記錄的在資產負債表上的無形資產和我們的想象很不一樣，一般資產負債表上有很大比重的無形資產是叫做Goodwill（商譽）。

### **1）Goodwill，商誉**

翻譯成商譽，一聽名字還以為是商家的信譽，比如鼎泰豐的信譽很好，是不是Goodwill商譽呢？不是，會計裡的Goodwill（商譽）非常具有迷惑性，Aswath認為Goodwill是最具危險性的一個會計科目。

要有Goodwill，必須有收購，而且是超出Book value賬面價值（賬面價值為總資產減去折舊)的收購，超出的部分就是商譽。比如下面的ABBV的资产负债表，2020年比2019年大增了1倍的Goodwill，就是因为abbvie收购了艾尔建，而且是溢價收購。

![](images/ABBV2.jpg)

### **2）Goodwill impairment商誉减值**

a）旧规则：Goodwill按照一定的年限（比如美股40年），每年减值1/40。

b）新规则：90年代末开始，GAAP和IFRS要求会计师每年评估Goodwill的价值看其有没有減值。

举例：波克夏公司每年都会评估其Goodwill的价值，巴菲特[2021年给股东的信](https://www.berkshirehathaway.com/letters/2020ltr.pdf)里有强调其2016年收购PCC出价太高导致波克夏损失100亿美金。

![](images/PCC.jpg)

可以看到波克夏2020的资产负债表里Goodwill一项，2020年比2019年大约減值100亿美金左右。

我猜这就是Aswath認為Goodwill是财务报表里最危險的一个会计條目的原因。

![](images/BRK1.png)

## **5.Current liabilities流動負債**

1）Non-interest-bearing liabilities,無利息的債務，大部分都是運營過程產生的，比如應付賬款等。

2）**interest-bearing short-term borrowing**,短期借貸和長期借貸中1年內要付的部分。

3）Defered revenue/salary/taxes/others，遞延稅收等其他短期內的付錢項目。

Defered revenue比如Netflix1年的合約，有半年是下個會計年度的，其中已收到錢的還沒有履約的部分就是Defered revenue。

當計算Non-cash working capital的時候，不計入interest-bearing short-term borrowing，而是把interest-bearing short-term borrowing計入到負債Debt的部分。

## **6.Debt due**

公司借貸有以下三種形式：

1）Corporate bond公司債券，在公開市場發行公司債。

2）Bank loan銀行借款。

3）Lease debt租賃負債。

2019年開始lease debt經營性的租賃負債也被要求記錄在資產負債表上。

## **7.Shareholder’s Equity**，**股东权益**

Aswath認為會計師越來越想用Shareholder’s Equity和market cap 競爭，作為估值工具。

1）par value

在資產負債表上常常可以看到，是歷史遺留，Aswath建議直接忽視。

2）公司年紀

一個公司的股東權益是這個公司歷年創造的價值之累積。所以年輕的公司的股東權益小，百年企業股東權益就大。

3）Capitalization effect

只有資本化的費用才會變成資產的一部分。會計師怎麼區分營業費用和資本支出會影響股東權益的大小。

比如下面netflix公司的損益表，它把科技研發算入Operating expense營業費用，但是Aswath認為這不是一個會計年度內的費用，應該算作資本支出。如果算作資本支出的話，資產就增加，相應的股東權益也會增加。

![](images/netfliex.jpg)

4）buyback effect

股票回購會大幅度的影響股東權益。

股票回購用了多少錢，股東權益就減少多少錢。因為很多公司市值是賬面價值的5到10倍，所以小小的回購就會消耗股東權益，不僅會讓股東權益為零，甚至為負數。

5）Equity股东权益為負值？

八分之一的美國公司的股東權益為負數。股東權益為負值，有如下两个原因：

- 有可能是因為公司還在創業階段，一直虧錢；

- 公司做了很多的股票回購;

除此之外，Aswath在課後習題裡提到還有另外2個導致股東權益為負的原因：

- 公司所付股息大於公司的淨收入Net income；

- 公司有重大的重組和特別支出（Restructuring and extraordinary charges）。

## **总结：**

关于怎么编写资产负债表，會計界有三种計價準則：

1）Record of capital invested

只是忠實的記錄而已。記錄所有在資產上的投資，比如土地、廠房、應收賬款等等所有在資產負債表上記錄的資產。

Damodaran教授是這一派的支持者。

2）Measure of current value

也就是fair value accounting公允价值計價，按市價計價，要求資產負債符合現在的市場價格。

這是目前的主流。

3）liquidation value

以清算的價格計價。

所以，即使是會計師群體對資產負債表怎麼評估都各持己見談不攏，何況是我們這些普通的投資者。

所以總體而言Damodaran教授認為資產負債表給我們的信息有些混亂。

注：本站所有學習達摩德仁教授課程的心得文章均已獲達摩德仁教授授權。
