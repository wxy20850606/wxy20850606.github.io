---
title: "學估值7.現金流折現法之折現率-part3 丟了CAPM beta，改用bottom-up beta"
date: "2021-05-21"
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

注：本文主要來自學習估值大师Prof. Aswath Damodaran（達摩德仁教授）的估值課程，課程詳情請點閱[視頻鏈接](https://www.youtube.com/watch?v=oi6M5KBWydg&list=PLUkh9m2BorqlJsEfix7R9jtSXClFZhGvC)和[相關資料](http://pages.stern.nyu.edu/~adamodar/)（Session7）

簡單回顧一下：

現金流折現法的折現率計算公式：

DCFE 股票自由現金流 折現率=權益資金成本=無風險利率+相對風險（比如Beta）\*股票風險溢價。

其中有三個組成部分：[無風險利率，股票風險溢](https://fulltimemammy.com/%e5%92%8c%e4%bc%b0%e5%80%bc%e5%a4%a7%e5%b8%ab%e5%ad%b8%e4%bc%b0%e5%80%bc5-%e7%8f%be%e9%87%91%e6%b5%81%e6%8a%98%e7%8f%be%e6%b3%95%e4%b9%8b%e6%8a%98%e7%8f%be%e7%8e%87-part1%e7%84%a1%e9%a2%a8%e9%9a%aa/)價，和相對風險（比如Beta）。

本文解釋Beta是什麼？為什麼不應該用beta，和怎麼計算相對風險-用bottom-up beta。

## **1.什麼是CAPM？**

**1.1CAPM是什麼？**

CAPM是一種資產定價模型,全稱**Capital Asset Pricing Model**，是用來計算個股或者一個投資組合的預期收益率。應用在投資組合中的目的是分散投資組合的風險。

我們投資股票市場，整體的預期收益率分為兩個部分：

一是可以保證的收益，即是無風險利率；二是買股票額外承擔風險的補償，即是股票風險溢價，（延伸閱讀：[無風險利率，股票風險溢價](https://fulltimemammy.com/%e5%92%8c%e4%bc%b0%e5%80%bc%e5%a4%a7%e5%b8%ab%e5%ad%b8%e4%bc%b0%e5%80%bc5-%e7%8f%be%e9%87%91%e6%b5%81%e6%8a%98%e7%8f%be%e6%b3%95%e4%b9%8b%e6%8a%98%e7%8f%be%e7%8e%87-part1%e7%84%a1%e9%a2%a8%e9%9a%aa/)）

市場預期報酬率=無風險利率+股票風險溢價（也可以理解為：市場預期報酬率=無風險利率+1\*股票風險溢價，因為市場的beta值為1）

因為個股波動的和市場不同，所以發明beta這個係數來做風險調整，計算個股的預期收益率。

**1.2什麼是Beta**

​Beta是其中CAPM模型中最重要的組成部分。Beta值描述的是一只股票和其所在的市場相比的波動程度。

把波動視為風險。波動越大，風險越大，預期收益就要越大，因為要補償風險。

市場指數的Beta值為1.個股的Beta值大於1表示其波動（風險）超過大盤，Beta值小於1表示波動（風險）小於大盤，Beta值等於1表示和大盤同時波動。

簡單來說，CAPM就是用Beta值作為風險參數來計算個股相對於市場（指數）的報酬。

![](images/21.jpg)

圖片https://finance.yahoo.com/quote/SPY?p=SPY

**1.3 CAPM模型個股預期報酬率計算**

因為單個市場的無風險利率都是一樣的，Beta係數可以調整的只有股票風險溢價。

個股預期報酬率=無風險利率+beta\*股票風險溢價

舉例：

台灣加權指數（0000）Beta值為1，台積電的Beta值為1.05，台幣無風險利率為-0.13%（按達摩德仁教授提供2021/01/01 數據），台灣股市股票風險溢價為5.31%（數值為2021/1/1）。

表示台積電幾乎和台灣股市同時波動。台積電預期報酬率=-0.13%+1.05\*5.31%=5.57%。

**1.4 CAPM模型投資組合預期報酬率計算**

投資組合預期收益率=無風險利率+投資組合beta值\*股票風險溢價。

投資組合beta=A股票beta\*A股票佔比+B股票beta\*B股票佔比+C股票beta\*C股票佔比。。。（可以無限延續）

以下這個60/20/20/20的投資組合收益舉例：

美元10年無風險利率為1.6%，美股股票風險溢價：4%，

SPY beta=1,佔比40%，AAPL beta=1.2佔比20%；Cocacola beta=0.62,佔比20%; Costco beta=0.64佔比20%.

這個投資組合的Beta值=1\*40%+1.2\*20%+0.62\*20%+0.64\*20%=0.892

這個投資組合收益為：1.6%+0.892\*4%=5.168%

意義：加入低beta值的公司，降低波動性，降低風險，不過也同時降低收益率。這和現在流行的股票和債券的投資組合配置的道理一樣。目的在於降低波動性降低風險。

## **2.Beta 怎麼計算？**

通過回歸分析Regression計算，把個股過去的股價和大盤指數的股價做回歸分析。回歸分析是分析兩個變量相關性如何。

![](images/image.png)

Rj是股票的收益，Rm為市場的收益。a為intercept截距，b是slope of regression。Slope 斜率就是beta值。

## 3.Beta的缺點。為什麼不應該用CAPM beta值來估值？

**2.1 回歸分析標準差很大的時候，beta值的範圍很大。**

回歸分析都有標準差，當標準差太大的時候，數據失真。

舉例：GPRO 的beta值為1.604，標準差為0.5，如果95%的信賴區間的話，beta值區間是多少？

答案是：95%的信賴區間是1.96個標準差（約2個標準差）。標準差為0.5，2個標準差為1. 那beta值的區間,最小為1.6-1=0.6，最大為1.6+1=2.6

可以看出beta值的可以從0.6到2.6，範圍很大，不確定性很大。

達摩德仁教授有統計美股上市公司beta的標準差的中間值是0.25，也就是一半公司beta的標準差大於0.25。

![](images/image-1.png)

來源：http://pages.stern.nyu.edu/~adamodar/

**2.2回歸分析效果很好，標準差很小也要警惕**

有可能是因為股票佔市場指數的比值很大。

下面這個Nokia的回歸分析，標準差很小，只有0.03，這種情況下beta值就沒什麼誤差。

原因是2000的Nokia的市值佔了HEX指數的80%。所以這個時候做回歸分析就等於在分析Nokia和Nokia自己而已。

就像台積電佔台灣指數的比例約為30%。這時候運行相關分析就等於運行自己和另一個長得很像自己的人，當然會得到比較接近於1的beta值，台積電的beta=1.05。

所以這個時候beta值的參考性就很值得考慮。

比如全世界的投資者投資台積電，為什麼要在乎台積電和台灣指數的相關關係呢？

![](images/image-2.png)

來源：http://pages.stern.nyu.edu/~adamodar/

**2.3 劇烈波動的股票的beta值也失真。**

2021/2月Gamestop股票劇烈波動，上演小散戶打敗華爾街大鱷魚的戲碼。但是其beta是-0.619. 表示和市場負相關，加入投資組合可以減低市場風險。

但是其股票劇烈波動，風險極大，和其負的beta值不匹配。

所以即使有很劇烈波動的股票，但是只要這些波動和市場沒有聯動關係，其beta值就很有可能為負，因為beta值是描述和市場的聯動關係。

![](images/image-3.png)

![](images/image-4.png)

**2.4 beta可以人為操控。**

1）可以通過截取起始時間來做回歸分析，不同的時間段得出的beta值不同。

2）raw beta vs adjusted beta

adjusted beta沒有任何的依據，只是簡單的做個加法讓它趨近與1.

20年前Bloomberg還會列出adjusted beta的公式：adjusted beta=0.67\*raw beta+0.33\*1。現在只是一個秘密。

**2.5 beta 是backward looking，是歷史，無法預測未來。**

## **3.巴菲特卡拉曼等價值投資者認為Beta值的缺點**

華爾街用波動性（beta）衡量風險，而巴菲特不是。巴菲特不把波動視為風險，巴菲特的風險定義是虧損的可能性（_**the possibility of loss or injury**_）。

華爾街用CAPM為依據來做投資組合管理，做分散投資，而巴菲特偏集中持股。巴菲特2020年底的持股中，光Apple就佔了40%。

3.1價值投資者對beta的觀點：

巴菲特舉了兩個例子來說明beta值的荒謬。

**1）beta值無法解釋同樣一只股票價格更便宜的時候為什麼beta值更大**。

**“a stock that has dropped very sharply compared to the market . . . becomes ‘riskier’ at the lower price than it was at the higher price”**

1973年華盛頓郵報市值80million，已經大大低於其內在價值400millon，但是當華盛頓郵報的市值從80million跌倒40million時，其beta值反而增加了。如果把beta值看為風險因子的話，為什麼價格更便宜反而風險更大呢？

**2）beta只考慮和市場相比的波動性，沒有考慮基本面**。

比如一個賣普通玩具的公司和賣芭比娃娃的公司，兩者和市場的波動性可能一樣，但是其公司獲利能力、毛利率絕對是差異很大的。beta無法從基本面的角度提供有意義的參考價值。

**“a single-product toy company selling pet rocks or hula hoops from another toy company whose sole product is Monopoly or Barbie.”** 

**3.2 Seth Klarman 論beta**

Seth Klarman在其著作Magin of Safety裡也提到了對beta的看法：

**1）beta沒有考慮基本面。**

“**_Beta views risk solely from the perspective of market prices, failing to take into consideration specific business fundamentals or economic developments_**_._“

**2）beta沒有考慮目前價格水平。**

**“ _The price level is also ignored, as if IBM selling at 50 dollars per share would not be a lower-risk investment than the same IBM at 100 dollars per share._”**

**3）beta忽視了類似巴菲特這樣的有影響力的投資者可以控制整個公司，影響公司的價值。**

“_Beta fails to allow for the influence that investors themselves can exert on the riskiness of their holdings through such efforts as [proxy contests](https://en.wikipedia.org/wiki/Proxy_contests), shareholder resolutions, communications with management, or the ultimate purchase of sufficient stock to gain corporate control and with it direct access to underlying value._”

**4）過去的價格波動無法預測未來的股票表現。**

_“_ **_The reality is that past security price volatility does not reliably predict future investment performance (or even future volatility) and therefore is a poor measure of risk._**_“_

總結一句話：價值投資者一般都不會使用CAPM beta.

## 4.影響相對風險的因素

那不用CAPM Regression beta怎麼計算某隻股票相對於市場的風險呢？

教授的思路是先看公司的生意，然後看營業槓桿率，最後再看財務槓桿率。

**4.1 Nature of product/service 生意的屬性**

有些產品，無論經濟好不好，我們都需要，像是油鹽醬醋這些生活必需品。有些產品特別會受景氣的影響，像是名牌包包，經濟不好的時候就先不買了。

所以，先考慮的是公司是提供什麼類型的產品或者服務。

**4.2 operating leverage 營業槓桿率**

如果把公司的營業成本分為固定成本和變動成本兩類的話，營業槓桿率就是衡量企業營運依賴固定成本的程度。比如AIRBNB 就比希爾頓酒店依賴固定成本的程度低，因為AIRBNB自己不用蓋酒店實際運營酒店。

營業槓桿率高的公司相對的beta值應該高。

（這點在操作上很難，因為很多公司不會在財務報表裡提供相關數據）

**4.3Financial leverage 財務槓桿率**

公司的財務槓桿率也會影響beta，因為債務越高風險越高，beta值越大。

具體操作上，達摩德仁教授使用的是Bottom-up beta來反應相對風險。

## 5\. 怎麼計算相對風險-Bottom-up beta：

不用單一歷史 Regression beta，而是用行業平均為基礎計算的bottom-up beta。

![](images/image-10-1024x582.png)

來源：http://pages.stern.nyu.edu/~adamodar/

**5.1 Step1：找出公司的生意類型**

比如台積電的業務只有一個是半導體，Amazon的業務有電子商務、訂閱服務、物流服務等。

一般公司的財報都會用Revenue或者Sales來分各個業務的收入。

下圖是Amazon的業務分類：

![](images/image-11.png)

**5.2 Step2:計算此業務的unlevered regression beta**

1）找到此業務的上市公司，越多越好，可以是全世界的公司，真找不到的話也可以找上下游的公司。

舉例：教授在分析Disney的電視業務的時候，發現美國的同類型的電視業務公司都是某個集團的一部分，很少有單獨只做電視廣播的。所以教授就用了其上下游公司來代替。因為他們受市場波動經濟波動的影響是一致的。

2）把這些公司的Regression beta計算出來，再簡單平均，得到levered regression beta。（簡單平均是避免有很大的公司佔權重太大使得數據像這個大公司靠攏；按過去歷史價格計算的Regression beta是已經考慮債務的 Levered beta）

3）用平均的債務權益比率（Debt to equity ratio），計算出unlevered regression beta。

**unlevered regression beta=levered regression beta/(1+(1-tax rate)(debt/equity))**

公式裡有（1-tax rate）是因為債務可以抵稅。

我覺得自己做回歸分析太麻煩也太難，可以直接找出一些同業務的公司，用雅虎財經提供的Beta值（為levered regression beta）再簡單平均就可以了。

**不過更棒的是教授貼心的在網站上提供給我們各個業務的unlevered regression beta adjusted for cash。我們自己只要從財務報表裡計算Debt to equity ratio就可以了。**

4）額外調整：根據目標公司在行業裡的營業槓桿率和產品屬性再次比較，調整unlevered beta值。舉例：台積電在半導體公司裡的毛利率算是很高，這樣的話公司的beta值應該在同行業來說應該是偏低的。（教授沒有再詳細說明，這部分要靠經驗積累）

**5.3 Step3:對於有很多業務的公司，計算每個業務的市值，得出按市值的比重。**

因為市場對不同生意的估值給的不同。

舉例：迪士尼的電影業務和其消費品販售業務所帶來的利潤估值是不一樣的。電影業務每賺的1塊錢更值錢。

教授用 是EV/Sales比，教授的網站也有提供不同產業的EV/Sales比可供使用。

**5.4 Step4:按照不同業務的市值權重，分別計算unlevered regression beta再加總，得出整個評估公司的unlevered beta 。**

具體計算方法：A業務**unlevered regression beta adjusted for cash**\*A業務市值比重+業務**unlevered regression beta adjusted for cash**\*B業務市值比重+C業務**unlevered regression beta adjusted for cash**\*C業務市值比重。。。。。。

額外考慮項目：如果可以預期未來某個業務會增長，某個業務會減少，還可以額外調整。

**5.5 Step5:計算Levered beta ，如果預期公司每年的債務權益比不同還可以每年調整beta值。**

Levered bottom-up beta=unlevered beta\*(1+(1-tax rate)(debt/equity))

額外考慮項目：如果公司業務類型很廣，且債務權益比率差別很大，這個時候就需要對每個業務按照債務權益比分別計算各個業務的levered beta。（不過難的是，我們一般投資人很難知道每個業務的債務權益比重。）

**5.6 Step6：計算cost of equity**

Cost of equity=無風險利率+levered bottom-up beta\*股票風險溢價

**5.7 cash adjusted beta**

如果公司的現金非常多的話，因為cash的beta值=0，還可以計算Cash adjusted beta。

按照Step4算出來的市值和公司目前的現金，做如下計算

cash adjusted beta=Levered bottom-up beta\*（市值/（市值+現金））+0\*\*（現金/（市值+現金））

## **6.bottom-up beta的優點：**

**6.1 很多beta平均可以得到更準確的beta值，減少標準差。**

bottom-up beta的標準差=beta標準差的平均值/公司數的平方根。

也就是說，如果有100家公司的話，標準差變成原來的十分之一。有9家公司的話，標準差變為三分之一。

![](images/image-12-1024x105.png)

**6.2bottom-up beta可以考慮不同的業務類型和公司的債務槓桿情況。regression beta只反應過去的歷史數據。**

**6.3bottom-up beta 可以評估剛剛ipo的公司。而regression beta不可以。**

## 7.作業:

按達摩德仁教授所教計算2021/5/18日計算台積電的Cost of equity如下：

![](images/image-8.png)

![](images/image-9.png)

## 6.總結

CAPM regression beta 是只用單一個股過去股價和市場股價相比較做出來的。標準差很高，拿美股來說，一半公司的Regression beta的標準差在0.25以上。用bottom-up beta 是用大量同業務類型的公司的Regression beta簡單平均，10家公司標準差可以變為1/3,100家公司可以變為1/10，這樣就可以得到整個業務和市場相比比較精準的變化程度。

Bottom-up計算步驟：

Step1：生意類型

Step2：此生意類型的Regression beta，因為此beta是考慮債務和現金的，所以要把此beta剔除債務和現金的影響，得出純業務本身的beta值，也就是**unlevered regression beta adjusted for cash**。（這步驟最難教授已經幫忙做了）

Step3：按生意的價值估算市值

Step4：按Step2和Step3得到的數據計算公司的unlevered bottom-up beta

Step5：考慮債務的影響把**unlevered** beta再還原為levered beta

Step6：考慮現金的影響把**levered** beta再還原為cash adjusted levered beta

有了替代的bottom-up beta，就可以計算公司的Cost of equity了。

Cost of equity=無風險利率+（cash adjusted）levered bottom-up beta\*股票風險溢價

其他參考資料：

教授的[Corporate finance](https://www.youtube.com/playlist?list=PLUkh9m2Borql2njENzmUX2DoZr5E2-YXs)，session6-10有更詳細闡述beta。

[how-warren-buffett-thinks-about-risk](https://www.vintagevalueinvesting.com/how-warren-buffett-thinks-about-risk/)

[Warren Buffett v. Modern Finance Theory](https://clsbluesky.law.columbia.edu/2013/04/15/warren-buffett-v-modern-finance-theory/)

[Value Investors on CAPM, Beta & Risk](https://www.gurufocus.com/news/156489/value-investors-on-capm-beta-risk)

Seth Klarman Margin of Safety

注：本站所有學習達摩德仁教授課程的心得文章均已獲達摩德仁教授授權。
