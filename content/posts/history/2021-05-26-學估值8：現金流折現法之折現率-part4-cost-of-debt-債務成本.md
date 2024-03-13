---
title: "學估值8：現金流折現法之折現率-part4 Cost of debt 債務成本"
date: "2021-05-26"
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

注：本文主要來自學習估值大师Prof. Aswath Damodaran（達摩德仁教授）的估值課程，課程詳情請點閱[視頻鏈接](https://www.youtube.com/watch?v=oi6M5KBWydg&list=PLUkh9m2BorqlJsEfix7R9jtSXClFZhGvC)和[相關資料](http://pages.stern.nyu.edu/~adamodar/)（Session8）

簡單回顧一下：

現金流折現法的折現率計算公式：

**DCFE 股票自由現金流 折現率=權益資金成本=無風險利率+相對風險（Bottom-up Beta）\*股票風險溢價。**

**DCFF(公司自由現金流) 折現率=資本成本=權益資金成本\*股權百分比+債務成本（1-tax rate）\*債務百分比**。

前面三篇文章分別學習了權益資金成本的三個組成部分：[無風險利率，股票風險溢價](https://fulltimemammy.com/%e5%92%8c%e4%bc%b0%e5%80%bc%e5%a4%a7%e5%b8%ab%e5%ad%b8%e4%bc%b0%e5%80%bc5-%e7%8f%be%e9%87%91%e6%b5%81%e6%8a%98%e7%8f%be%e6%b3%95%e4%b9%8b%e6%8a%98%e7%8f%be%e7%8e%87-part1%e7%84%a1%e9%a2%a8%e9%9a%aa/)，和[相對風險（](https://fulltimemammy.com/%e5%ad%b8%e4%bc%b0%e5%80%bc7-%e7%8f%be%e9%87%91%e6%b5%81%e6%8a%98%e7%8f%be%e6%b3%95%e4%b9%8b%e6%8a%98%e7%8f%be%e7%8e%87-part3-%e4%b8%9f%e4%ba%86capm-beta%ef%bc%8c%e6%94%b9%e7%94%a8bottom-up-beta/)[Bottom](https://fulltimemammy.com/%e5%ad%b8%e4%bc%b0%e5%80%bc7-%e7%8f%be%e9%87%91%e6%b5%81%e6%8a%98%e7%8f%be%e6%b3%95%e4%b9%8b%e6%8a%98%e7%8f%be%e7%8e%87-part3-%e4%b8%9f%e4%ba%86capm-beta%ef%bc%8c%e6%94%b9%e7%94%a8bottom-up-beta/)[\-up Beta）](https://fulltimemammy.com/%e5%ad%b8%e4%bc%b0%e5%80%bc7-%e7%8f%be%e9%87%91%e6%b5%81%e6%8a%98%e7%8f%be%e6%b3%95%e4%b9%8b%e6%8a%98%e7%8f%be%e7%8e%87-part3-%e4%b8%9f%e4%ba%86capm-beta%ef%bc%8c%e6%94%b9%e7%94%a8bottom-up-beta/)。

本篇學習折現率的最後一個組成部分：Cost of debt，即債務成本。

## **1.什麼是Debt？**

一般我們會認為債務就是銀行貸款。其實會計和企業金融上認為的Debt不僅僅是貸款Loan。

公司融資有兩種方式，一種是Equity，一種是Debt。那怎麼把二者區分開來呢？主要是看Debt的如下特征。

**1.1Debt的三個特征：**

1）contractual commitment

以合同的形式承諾未來要付一定的款項。

2）tax deductible

貸款一般都可以抵稅。除了在少數幾個國家不可以外，在大部分的國家都可以抵稅。

3）lost of control

不能按合約付款會導致不可控制的風險，比如失敗、破產。

**1.2 資產負債表裡的Liabilities全部都是Debt嗎？**

不是，Debt全部都是Liablities， 但是liablities不全是Debt。

1）Bank loan、long term bank loan

是。

2）Corperate bond

是。

3）Short term bank debt

是。很多投資銀行計算Debt to equity ratio時只算入長期貸款，忽略短期貸款。其實二者都需要計算。

4）Account payable、Supplier credit是Debt嗎？

不是。應付賬款沒有很明確的利息。雖然說直接現金付款有可能可以打折，但是這個是隱含的不是直接的利息。

雖然應付賬款和供應商信貸不是Debt，如果數額太大，它們還是有可能導致公司破產。

5）Operating lease營業租賃，captial lease融資租賃是Debt嗎？

是。2018年，lease 佔了80%的表外債務。從2019年開始，GAAP和IFRS才要求會計師們把所有的lease都當成debt。

在這之前，只有Captial lease融資租賃才處理為Debt，Operating lease營業租賃都處理為營業費用，這部分之前都是表外債務。

## **2.債務成本Cost of debt是什麼？**

**2.1 債務成本是公司今天取得長期貸款的利率。**

重點有兩個：

1）現在。

不要用1年前，2年前的貸款成本，也不是1個月前的，甚至不是1天前的，而是今天的。是今天可以取得長期貸款的利率。

2）長期貸款。

公司也可以借短期貸款來資助公司的項目。達摩德仁教授認為短期貸款到期了可以再借短期貸款，然後一直循環下去。對於這些短期貸款，也可以計算其長期的利率。

**2.2 債務成本分成兩個部分**

1）是無風險利率，即不承擔風險可以獲得的利率；

2）額外的default spread違約利差，就是對公司萬一還不上錢這個違約風險做的補償。

## **3.default spread違約利差怎麼計算？**

**3.1 最簡單且優先的方法——企業評級**

約10%-15%的公司有評級。可以用GOOGLE直接搜索。

Google "TSMC bond rating“可以搜到台積電的企業評級為AA3(Moody‘s）。

注意要搜索的是企業的評級，而非企業的某個企業債的評級。

二者不同的地方在於評級沒有很高的企業也可用其最好的資產來擔保企業債，使得其企業債獲得比較高的評級。

有三大評級公司 Moody's, S&P, Finch. 如果三大評級公司給予的等級不一樣，用最新的評級。

**3.2 沒有評級的公司怎麼計算違約利差**

達摩德仁教授研究了2000家有評級的公司後發現，評級中有50%的影響來自於Interest coverage ratio。

![](images/image-15.png)

對於沒有評級的公司，可以用計算Interest coverage ratio（利息保障倍數）來推導它相應的評級是什麼。

（需要注意的是，如果公司在新興市場，無論公司大小， 都需要用小市值公司的評級。）

![](images/image-13.png)

來源：http://people.stern.nyu.edu/adamodar/New\_Home\_Page/datafile/ratings.html

**3.3 EBIT小於0的新創公司怎麼估計評級？**

達摩德仁教授建議對於這些新創公司就直接用BB評級。因為這些公司一般都沒有債務，即使有債務也非常非常少，沒有必要多花力氣研究評級。

其中的country default spread可以根據具體的情況分配比重。例如巴西的Embraer公司因為總部在巴西，因為其業務九成來自於歐美，所以教授分配了2/3的country default spread。

**3.4 舉例 台積電TSMC評級為AA-,查詢列表裡Spread為0.85%**

2021年3月台積電的前12個月EBIT為21.277bUSD，同期所付利息為63.4m USD，所以其利息保障倍數為335。

遠遠大於AAA級別的利息保障倍數8.5.為什麼其企業評級不是AAA呢。是因為評級公司默認企業評級不能高於國家評級。台灣的主權評級為AA-,所以台積電的評級被台灣的主權評級拖累了。

![](images/image-16.png)

**3.5 評級和違約利差對應表：**

[Ratings, Interest Coverage Ratios and Default Spread](http://people.stern.nyu.edu/adamodar/New_Home_Page/datafile/ratings.html)。達摩德仁教授每年都會更新數據，免費開放給所有的人使用。

## 4.怎麼計算Cost of debt債務成本

4.1 對於Country default Spread低的國家，比如歐美

**pre-tax Cost of debt=Risk free rate+company default spread**

教授的違約利差表，是用的美元來計算美國市場的數據得到的。

4.2對於Country default Spread高的高風險國家，比如巴西，越南等

**pre-tax Cost of debt=Risk free rate+country default spread+company default spread**

其中的country default spread可以根據具體的情況分配比重。例如巴西的Embraer公司因為總部在巴西，因為其業務九成來自於歐美，所以教授分配了2/3的country default spread。

## 5.債務的減稅效應，cost benefit of debt

**5.1債務融資相對於股權融資來說，具有減稅效應。**

因為債務融資需要負擔的利息作為財務費用可以在稅前利潤前扣除（如果公司有繳稅的話）。

複習下[損益表](https://fulltimemammy.com/%e7%82%ba%e4%ba%86%e6%8a%95%e8%b3%87%e5%ad%b8%e6%90%8d%e7%9b%8a%e8%a1%a8-%e5%92%8c%e4%bc%b0%e5%80%bc%e5%a4%a7%e5%b8%abaswath-damodaran-%e5%ad%b8%e7%bf%92%e6%9c%83%e8%a8%88%e5%bf%83/)，損益表營業收入減去銷貨成本得到毛利潤，毛利潤減去營業費用得到營業利潤，**營業利潤減去財務費用得到稅前利潤。**

![](images/%E6%8D%9F%E7%9B%8A%E8%A1%A8.jpg)

**5.2 Tax benefit 計算**

Tax benefit=tax rate\*interest income

**5.3 用什麼稅率？**

答案是：marginal tax rate邊際稅率，

美國的企業所得稅聯邦級別2017年從最高35%（分為4級）降低為21%，再加上州級別的稅，總共大約24%-25%之間。

台灣邊際稅率2018年開始從17%調到20%。

更多國家請看：[Corporate Marginal Tax Rates - By country](http://pages.stern.nyu.edu/~adamodar/New_Home_Page/datafile/countrytaxrate.html)

關於不同稅的區別請參考：[從財務報表看公司繳了多少稅，邊際稅率、有效稅率、遞延所得稅、遞延所得稅資產/負債解釋](https://fulltimemammy.com/%e5%be%9e%e8%b2%a1%e5%8b%99%e5%a0%b1%e8%a1%a8%e7%9c%8b%e5%85%ac%e5%8f%b8%e7%b9%b3%e4%ba%86%e5%a4%9a%e5%b0%91%e7%a8%85%ef%bc%8c%e9%82%8a%e9%9a%9b%e7%a8%85%e7%8e%87%e3%80%81%e6%9c%89%e6%95%88%e7%a8%85/)

## **6.after-tax cost of debt**

**after-tax cost of debt=pre-tax cost of debt\*（1-tax rate）**

舉例：台積電，以美元計算。

pre-tax Cost of debt=Risk free rate+company default spread=1.58%+0.85%=2.43%

after-tax cost of debt=2.43\*（1-20%）=1.94%

## **7.Cost of Hybrids混合融資方式融資成本**

除了公司最主要的兩種融資方式equity和debt之外，有些公司還有混合型的融資方式，比如可轉換債券和優先股。

**7.1 Convertible debt可轉換債券**

可轉換債券，本身是債券，但是在一定的條件下可以轉換成股票。

所以達摩德仁教授認為可以把可轉換債券分開，分為股權和債務，在合併到所有的股權和債務裡一併計算。

舉例：A公司發行面值125萬美金的可轉換債，債息為4%，到期日為10年，市值為140萬美金。如果公司的評級為A，A級的直接債券利息為8%。

債權部分（straight debt）=（4% of 125）（PV of annuity，10years，8%）+125/1.0810\=91.45million

股權部分=140-91.45=48.55million

**7.2 Preferred stock優先股**

優先股是按優先約定好的股息率領取股利的股份。

一般公司不會用優先股融資，因為優先股的固定的股利不能抵稅。

但是銀行和新創公司很喜歡用優先股融資。

優先股的成本=preferred dividend yield=preferred dividend per share/market price per preferred share

如果評估新創公司，而且其優先股融資佔比很多（大於5%），這時候就需要在增加一個項目了。

資本成本=股權資金成本\*股權佔比+債務成本\*債權佔比+優先股資金成本\*優先股佔比。

## 總結：

債務成本計算方法：

**pre-tax Cost of debt=Risk free rate+company default spread**

after-tax cost of debt=pre-tax cost of debt\*（1-tax rate）

其中**company default spread**可以通過查詢企業評級再從達摩德仁教授更新的對應表裡查詢利差數字。對於沒有查不到評級的公司可用利息保障倍數估算評級。

其他參考資料：

教授的[Corporate finance](https://www.youtube.com/playlist?list=PLUkh9m2Borql2njENzmUX2DoZr5E2-YXs)課程，session11有更詳細闡述cost of debt。

[Session 5: Accounting Inconsistencies](https://www.youtube.com/watch?v=un6-H_wOizM&list=PLUkh9m2BorqmKaLrNBjKtFDhpdFdi8f7C&index=9)

[https://www.investopedia.com/terms/d/debt.asp](https://www.investopedia.com/terms/d/debt.asp)

[https://en.wikipedia.org/wiki/Bond\_credit\_rating](https://en.wikipedia.org/wiki/Bond_credit_rating)

[http://people.stern.nyu.edu/adamodar/pdfiles/acf3E/presentations/capstruchoices.pdf](http://people.stern.nyu.edu/adamodar/pdfiles/acf3E/presentations/capstruchoices.pdf)

注：本站所有學習達摩德仁教授課程的心得文章均已獲達摩德仁教授授權。
