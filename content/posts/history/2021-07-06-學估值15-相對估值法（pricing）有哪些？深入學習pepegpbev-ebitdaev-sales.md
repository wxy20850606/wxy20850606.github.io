---
title: "學估值15 相對估值法（Pricing）有哪些？深入學習PE,PEG,PB,EV/EBITDA,EV/Sales各指標背後的驅動因素"
date: "2021-07-06"
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

注：本文主要內容來自學習估值大师Prof. Aswath Damodaran（達摩德仁教授）的估值課程，課程詳情請點閱[視頻鏈接](https://www.youtube.com/watch?v=oi6M5KBWydg&list=PLUkh9m2BorqlJsEfix7R9jtSXClFZhGvC)和[相關資料](http://pages.stern.nyu.edu/~adamodar/)（Session18-21）。

本文所有公式和圖表均來自與：[http://pages.stern.nyu.edu/~adamodar/](http://pages.stern.nyu.edu/~adamodar/)。

本來在想學了DCF估值就不用學Pricing了，但是想想我之前接觸到的99%的股票分析都是Pricing。所以為了增加自己未來看投資報告的辨識能力，決定好好學習整理下相對估值法。

## **1.相对估值法是什么？**

**1.1定義**

相對估值法（Pricing）是通過比較市場給其他相似的資產的估值倍數，確定出一個適當的倍數來給目標資產估算價值。

1）參考目前市場價值或者過去歷史價格。

2）和相似資產比較獲得目標資產價值。

**1.2相對估值法理解**

相對估值法很多種，讓人頭暈目眩。其實它們都是使用絕對的倍數，本質上都是分式。

分子是付出的價錢，分母是獲得的回報（現在的或者是未來的）。

分子可以是每股股價、公司股權市值、公司市值、運營資產市值等等。

分母可以是營收、EBIT、現金流或者賬面價值等等。

比如P/E，分子是每股股價，分母是獲得多少Earnig，PE=78,表示每股股價=78\*Earning

比如EV/EBITDA,分子是公司總價，分母是獲得多少EBITADA(現金流），EV/EBITDA=15，表示Enterprise value=15\*EBITDA

比如EV/Sales,分子是公司總價，分母是獲得多少銷售額，如果EV/Sales=5，表示Enterprise value=5\*sales。（因為新公司尚未獲得金錢回報，使用者是未來賺錢可能。

![](images/image-92-1024x487.png)

來源：http://pages.stern.nyu.edu/~adamodar/

**1.2 相對估值的計算邏輯**

1）找出相似資產及其市場價。

2）找到適合的絕對倍數。

3）對比差異，通過對比差異，得出低估還是高估。

**1.3相對估值法和內含價值法（DCF現金流折現法）比較**

1）驅動因素不同

DCF估值的驅動因子是自由現金流量、成長率和風險。估值變化相對較慢。

相對估值法是建立在市場價格的基礎上，市場價格比較受到市場先生的每天的情緒影響，也會受到消息面的影響。估值有可能急速變化。

2）顯性VS隱性

DCF折現法迫使我們估計公司的營收、利潤率、成長率、再投資率，得出公司的自由現金流量，然後再根據風險折現到今天。

而相對估值法表面上用倍數估值，但是沒有兩個一模一樣的公司，想要做出一定的調整，還得從基本面找答案，所以現金流量成長率和風險也會隱含在我們給的倍數裡面。

3）結果不同

DCF估值的結果是絕對價值，不受市場的情緒影響。

相估值的結果是價格相對低估或高估，是建立在整個行業的估值是正確的基礎上。如果整個行業高估，就會水漲船高，不知道水會不會退潮，會退到哪裡。

4）變通程度不同

DCF估值只能通過計算未來自由現金流量，在折現到今天這一種方式。

相對估值花樣很多，理論上可以有無限的倍數可以運用。

Earning為負，沒辦法用P/E；但是sales為正，可以用EV/sales（比如TESLA）；如果連sales也沒有，可以發明新倍數，比如用EV/Users(比如Twitter）。

5）DCF估值是故事和數字的結合，而相對估值完全是相信市場，利用市場數據在做數據分析。

> The nature of pricing is you trust the crowd.
> 
> But the crowd should be respected but they can make very big mistakes.

**1.4 為什麼大家喜歡用相對估值法**

85%的專業分析師使用相對估值法，50%的併購採用相對估值法，業餘投資人幾乎都是相對估值法，甚至自稱價值投資者的也很少真的使用DCF來計算intransic value。原因如下：

1）簡單省力

不用費腦筋，用個Yahoo finance不要一分鐘就可以看出來公司目前各種倍數。

2）減少被找茬

試想分析師如果用DCF，把各種數據的細節比如成長率、利潤率、折現率拿出來，會被各種質疑。

但是用倍數，會比較籠統，沒什麼細節，很難挑毛病。

3）從眾行為

分析師分析報告buy佔90%，sell佔10%。

用相對估值，總能找到相對低估的股票。分析師每天有新的股票推薦購買。

用DCF找到低估的股票很難，機會沒有那麼多，如果每天的報告都是Sell，就會變成最孤獨的存在。

**1.5 使用相對估值請注意**

1）歷史經驗不可靠

2）分析背後的隱含驅動因素

3）自己計算，不要依靠財經網站.

因為你不知道他們運算使用的具體數據是什麼。

## 2.達摩德仁教授的相對估值步驟

**2.1定義倍數**

要清楚的了解倍數是怎麼計算的。分子具體是什麼，分母具體是什麼。

**1）倍數的分子和分母有沒有保持一致？**

公司的投資者分為股權投資者和債務投資者。

如果分子的價格是股權價值的話，分母就應該是股權的收益或者是股權的賬面價值。

如果分子的價格是整個公司（股權加債權）價值的話，分母就應該是整個公司的收益或者賬面價值。

考慮一致性主要是為了保證分子和分母聯動，避免有任何因子只影響分子或者只影響分母導致倍數喪失參考價值。

舉例：

P/E, 分子為Price per share，是每股股權價值；分母為Earning per share，不是所有的Earning，而是Net income/shares, 而Net income是股權投資者收入（不包含投資收入）。所以一致。

EV to EBITDA，分子為Market value of operating assets，父母為Earning of operating assets，一致。

![](images/image-5-1024x88.png)

Price to EBITDA ，分子為每股股權，分母為Earning of operating assets，是營運資本的收益，而非股權收益，就不一致。

Price to sales，分子為每股股權，分母為sales或Revenue，但是營收是屬於整個公司的收益，不僅僅是股權投資者。所以不一致。很多分析師使用Price to sales，尤其在科技公司，因為科技公司的負債很少，所以還勉強湊合使用，如果負債比很高的公司用Price to sales就會大錯特錯。

2）各公司一致。

各種相對估值倍數都是建立在會計數字是可靠的並且一致的基礎上，但是即使在不同的會計準則下，各個公司的會計做賬的方式也有差別。比如同為研發支出，公司可以選擇把它列入費用，也可以選擇列入資本支出。

**2.2描述倍數**

有很多分析師會按經驗來總結：10倍Earning很便宜，6倍EBITDA很便宜，1倍Book value很便宜。

但是這樣會存在偏見。

要用倍數的分佈（Distribution）來判斷低估還是高估，而不是經驗。比如你可以說第25分位的PE是比較便宜比較低估的。

當聽到分析師說平均PE的時候也要小心，如下圖，美股2021年PE平均值為109.79（平均值被一個PE為36157的公司拉大了），而PE的中間值為18.15

![](images/image-12.png)

來源：http://pages.stern.nyu.edu/~adamodar/

下圖是2021年全世界的PE分佈圖，其中美股第25個百分位是11.5，表示PE11.5是比較低的，中位數是40，，只有32.39%的公司PE大於0，即有正的earning。

所以統計的樣本只有32%的公司，又犯了以偏概全的錯誤。

![](images/image-3-1024x602.png)

from：http://pages.stern.nyu.edu/~adamodar/

**2.3分析倍數背後的驅動因素**

1）思考兩個問題

**倍數後面有哪些驅動因素？**

**這些驅動因素怎麼影響我們的倍數？**

比如PE=10,表面上看很便宜，但是我們要分析是什麼原因導致PE這麼低，判斷是真的低估，還是真的只值這麼多錢。

2)怎麼找到驅動因素

思路：分子是付出的價錢，通常是股價或者公司價值，這可以利用DCF現金流折現法算出，再用等式兩邊都除以分母，得到各個倍數的內含相對價值公式。

不過我們獲得公式的目標是找到影響的因子，而不是為了計算。

以PE為例：

Step1：用簡單的股息模型（Gordon growth model）計算股價公式如下, DPS1為下一年的股息，r為cost of equity，gn為growth rate

![](images/image-17.png)

Step2 公式兩邊都除以Earning，其中DPS/EPS=Payout Ratio即配息率。

![](images/image-16.png)

由上述公式可以看出，**影響成熟穩定配息公司PE的三個因子是Payout ratio，cost of equity 和growth rate。**

Step3,如果有公司不發股息，可以將股息模型換成潛在股息（FCFE）。

![](images/image-18.png)

Step4，如果想分析高增長的公司

![](images/image-19.png)

![](images/image-20.png)

**影響成長公司PE的三個因子是Payout ratio，cost of equity 和growth rate。**

2.4 運用倍數並且根據差異調整

我們的問題是，一有倍數就根據腦中的經驗直接使用，沒有清楚分析背後的驅動因素。

1）怎么找到相似资产

樣本多的話，優點是大數定理，樣本越多，越能準確找到背後的規律。缺點是非相似資產

樣本少的話，優點是資產相似，缺點是樣本會比較少。

2）怎么根据差异调整

這一點教授給出的解決方案是用數據分析，特別是回歸分析，找個各個變量對倍數影響的具體數值，把差異具體量化。

下面開始單獨學習各個常用的選股指標倍數。

## 3.PE

3.1 定義：PE是什麼？

1）P/E是Price per share除以EPS( earning per share)。

其中：

Current PE: EPS in recent financial year（最近財政年度的財報，比如台積電最近的是2019財年）

Trailing PE: EPS in trailing 12 months(最近的12個月的Earning）

Forward PE: EPS for next 12 months（預估未來的12個月的Earning）

2）EPS 為Net income除以股數。對於大量發股票薪酬（尤其是option）的科技公司來說，有如下三種EPS：

Primary EPS(outstanding shares）

Fully diluted EPS(shares+all options）

Partially Diluted EPS(Shares+in-the-money options）

因為Price是市場價格，市場已經知道公司有發這麼多的股票薪酬，所以已經把Options列入計算中了，所以為了分子分母一致，也為了避免計算Option的麻煩，最好用如下的公式計算PE,

**PE=Total market cap/Net income**

3.2 影響因子：

![This image has an empty alt attribute; its file name is image-20.png](images/image-20.png)

通過以上公式，得出**影響成長公司PE的三個因子是Payout ratio，cost of equity 和growth rate。**

1）Growth rate變動怎麼影響PE?

很明顯，成長率越高，PE越高，兩者正相關。

當上市公司有Earning report出來的時候，如果成長超預期，股價會大漲；成長沒有達到預期，股價會大跌。因為目前的無風險利率很低，未來的增長價值更高。所以股價對成長的反映更為敏感。

2）Cost of equity中的beta值如何影響PE?

可以看到beta值越高，風險越高，PE越低，這表示有時候不能一味的追求高成長率，也要把風險控制在一定的水平，這樣才能得到市場最佳的估值。

![This image has an empty alt attribute; its file name is image-21.png](images/image-21.png)

from：http://pages.stern.nyu.edu/~adamodar/

高增長的公司有高的PE，低增長的公司有低的PE。如果只選PE值很低的公司的話，會忽略幾乎大部分的高成長型的公司。

所以彼得林奇提出要不僅要看PE，更要看成長率，於是把PEG指標發揚光大。

## 4.PEG

4.1**公式：**

![](images/image-23.png)

4.2**含義：**獲得每1%的增長率付出多少倍的PE。數值越低，表示投資者為1%的成長率付出的代價越低。

我們都喜歡高成長，但是怎麼控制我們付出的代價，發明PEG指標就是這個目的。

比如PE=20， Growth rate=20%，PEG=1，表示我們為了1%的成長率付出的代價是1倍Earning.

比如PE=30, Growth rate=10%，PEG=3，表示我們為了1%的成長率付出的代價是3倍的Earning.

投資者的目標是付出更少的錢獲得更高的成長率，所以會選擇比較低的PEG。

就我潛水Seeking alpha多年的經驗：好像很多分析師說合理的估值，成長率多少，PE多少，即很多人認為PEG=1是比較合理的（不一定正確）

4.3**驅動因素**：

利用兩階段DCF估值模型，計算P:

![](images/image-24.png)

分式兩邊除以增長率，得到如下複雜的公式。

![](images/image-25.png)

影響因子為：

**1）PEG和Beta值。**

Beta值描述的是一只股票和其所在的市場相比的波動程度。經濟學中把波動視為風險。

相關閱讀：[學估值7.現金流折現法之折現率-part3 丟了CAPM beta，改用bottom-up beta](http://學估值7.現金流折現法之折現率-part3 丟了CAPM beta，改用bottom-up beta)

可以看到[Beta](https://fulltimemammy.com/%e5%ad%b8%e4%bc%b0%e5%80%bc7-%e7%8f%be%e9%87%91%e6%b5%81%e6%8a%98%e7%8f%be%e6%b3%95%e4%b9%8b%e6%8a%98%e7%8f%be%e7%8e%87-part3-%e4%b8%9f%e4%ba%86capm-beta%ef%bc%8c%e6%94%b9%e7%94%a8bottom-up-beta/)值越高，PEG值越低。

忽略風險，高成長也高風險，風險更高的公司有比較低的PEG。

![](images/image-29.png)

from：http://pages.stern.nyu.edu/~adamodar/

**2）PEG 和Retention ratio，ROE**


下圖表示：假設成長率不變，Retention越高，PEG越低。

同樣的成長率，Retention ratio越高，表示ROE越低。

**ROE越低PEG越低。ROE越高，PEG越高。**

從這一點看挑選低PEG值的公司有可能低ROE的公司，即資金運用效率不高的公司。

![](images/image-26.png)

from：http://pages.stern.nyu.edu/~adamodar/

比如說，A和B兩公司去年都賺了100萬，今年都會成長40%，即賺40萬。

A公司分給股東50萬，自留50萬，利潤留存率：50%，所以40%=50%\*ROE，所以ROE=8（Growth rate=Retention ratio\*[ROE](https://fulltimemammy.com/roeroc-roicroacash-flow-roiccfroi/)）

B公司一塊錢都不分給股東，自留100萬，利潤留存率：100%, 所以40%=100%\*ROE. ROE=4

但是計算PEG指標的時候，A公司的PEG會比較高，B公司的PEG會比較低。

這導致我們選PEG低的公司會選到賺錢比較沒有效率的B公司。

**3）PEG和Growth rate**

成長率和PEG比率的關係是U型，表示成長率增加可以使得PEG增加，或者使得PEG減小。這會導致我們無法判斷。

![](images/image-27.png)

from：http://pages.stern.nyu.edu/~adamodar/

使用PEG的時候，我們比較很多公司然後選擇比較低PEG的公司時，要確保我們的公司：

- 風險沒有比較高。因為風險很高的公司PEG值很低，這會誤導我們。
- ROE沒有比較低。因為ROE低的公司PEG值低，會誤導我們。
- 成長率，一開始成長率增加PEG會降低，但是成長到達一個點以後PEG又會上升。同一個行業裡有最低的PEG值的往往成長率保持在行業中間值。而有最高的成長率或者最低的成長率的公司往往PEG值比較高。

## 5.P/B，Price to book,市淨率

很多價值投資者很喜歡市場價值低於Book value的公司，以為把公司破產清算的話可以獲得清算價值和賬面價值的價差。但是現實中不見得如此。

5.1定義：

市淨率是每股股價除以每股淨資產。也可以用股票市值除以資產減去負債的差額

P/B=Price per share/book value of equity

或者P/B=Market value/(book value of assets-book value of liabilities)

5.2影響因子分析：

![](images/image-30.png)

![](images/image-31.png)

因為Growth=(1-payout ratio）\*ROE，所以上述公司可以簡化為：

![](images/image-32.png)

所以影響因子為：ROE,成長率，股息配發率和cost of equity。

**其中，ROE是決定性的影響因子，ROE越高，PB越高。**

注意：

- 當ROE約等於Cost of equity差不多的時候，P/B=1
- 當ROE大於Cost of equity時，P/B>1
- 當ROE小於Cost of equity是，P/B<1

當有人說某公司P/B=0.5很便宜的時候，第一個要研究的就是這個公司的ROE是不是很低，因為P/B值很低的公司往往都是ROE很低的公司。

## 6.EV/EBITDA

目前有近40%的分析師使用EV/EBITDA代替P/E,Youtube上很多CFA也是很喜歡用EV/EBITDA,也有可能人人都知道PE，但是不見得人人都知道EV/EBITDA，分析師用EV/EBITDA顯得貌似更專業吧。

6.1定義

![](images/image-8-1024x92.png)

1）分子有減去Cash，是因為現金帶來的收入在損益表中是列在Operating income下面的Other income欄目裡；分母的EBITDA只包含operating income而沒有包含other income。為了一致，分子要減去Cash。

2）對於有很多Cross Holding的公司，

Minority holding（沒有對營運有影響或者持股小於50%）帶來的收入也是列在Operating income下面的Other income 裡面，分母不含此部分收入，為了保持一致，需要將分子減去Minority holding的市值。

Majority Holding（有營運控制權或者持股大於60%），要把控股公司100%的收入列入Operating income，最後再從Net income中用Non-controling interest 把不屬於自己的部分減去。如果真要保持一致的話，分母中包含了100%的Majority Holding的收入，分子就就應該加上100%的Majority Holding的市值。（超級複雜，除非選擇視而不見）

下圖的損益表供參考。

![](images/image-9.png)

6.2 影響因子分析

![](images/image-38.png)

![](images/image-39.png)

![](images/image-40.png)

影響因子：

1)Tax rate,

和PE不同，因為PE的分母是Earning，來自於Net income，是稅後的，但是EBITDA是稅前的。所以稅收會影響EV/EBITDA

2)Cost of Capital

3)成長率

4）Reinvestment rate（Or ROC）

- 稅率越高，EV/EBITDA越低。
- 成長率越高，EV/EBITDA越高。
- 資本成本越高，EV/EBITA越低。因為資本成本高表示企業風險大， 當然估值較低。
- 再投資率越高，EV/EBITDA越低。為了增長需要投入非常多的錢，表示資金的使用效率不高，所以估值較低。
- ROC，資本報酬率越低，EV/EBITDA越低。

## 7.EV/SALES

7.1計算：

![](images/image-53.png)

7.2影響因子分析：

![](images/image-37.png)

影響因子：

1）成長率

2）資本成本

3）再投資率

各個倍數指標影響因子匯總：

![](images/image-42.png)

from：http://pages.stern.nyu.edu/~adamodar/

## 8.怎麼用？

因為很多人包括教授課堂裡的學生不會運行或者懶得自己運行回歸分析，所以教授按市場給我們做好了回歸分析，短期來說我們直接套公式計算就好了。長期來說，還是要學統計、數據分析等相關知識。我們沒有數據庫不見得可以運行全市場的回歸分析，但是好好學一下運行幾十家相似公司或者同行業公司的回歸分析應該是可能的（列入以後學習清單）。

註：下面個表的R2表示的可以解釋的部分。

比如對於美國來說，PE=4.1+1.71Beta+17.4payout+120.4expected growth，後面的R2\=39.4，意思是這個公式可以解釋市場上40%的個股情況。

1）PE

![](images/image-43.png)

from：http://pages.stern.nyu.edu/~adamodar/

2)PEG

![](images/image-44.png)

from：http://pages.stern.nyu.edu/~adamodar/

3)PB

![](images/image-45.png)

from：http://pages.stern.nyu.edu/~adamodar/

4)EV/EBITDA

![](images/image-46.png)

from：http://pages.stern.nyu.edu/~adamodar/

5) EV/SALES

![](images/image-47.png)

from：http://pages.stern.nyu.edu/~adamodar/

6) EV/Invested Capital

![](images/image-48.png)

from：http://pages.stern.nyu.edu/~adamodar/

## 9.這麼多指標，到底該選擇哪一個？

1）Enterprise, Equity or Firm value？

銀行行業使用股權類的倍數。債務槓桿很大使用市值類的倍數。

2）Revenues, Earnings, Cash Flows or Book value?

在確保樣本數夠多的情況下，每個都可以。但是如果行業裡大部分都還沒有賺錢，此時未來避免損失大量樣本，可以使用Revenue。

3）Current, Trailing, Forward?

都可以，確保一致就行了。盡量找到可以解釋目標公司和比較樣本之間差異最大的指標。

4）樣本大小？

找到相似資產和樣本數量之間的平衡。

5）舉例分析：

以輝瑞Pfizer為例，選擇EV/Invested capital，因為我暫時不會自己回歸分析樣本，量化相關因子的影響，所以我挑選教授提供的最大R2的指標。

另外，分析輝瑞的時候我最不喜歡的就是其資本成本ROIC太低，我想選擇有此因子的倍數指標。

![](images/image-54.png)

得出：IC=139395, DFR=0.5785， g=11.5%（此為未來5年的復合成長率，今年55%，明年5%，後年之後2%）， ROIC=7.49%

EV/IC=4.29-4.02\*0.5785+2.1\*11.5%+6\*7.49%=2.65533

EV=139395\*2.65533=**370139million$**

今天輝瑞 EV=Market cap+debt-Cash=222396+39699-16985=**245110million$**

用此指標分析，輝瑞被低估。

## 10.總結：

1）相對估值是相對的，難以避免全市場或者全產業被低估或者被高估的風險。

比如一個60公斤的人和80公斤的人比顯得很瘦，但是站在40公斤的人面前，會顯得很胖.

2）沒有絕對相同的資產，每個資產都是獨一無二的。所以放棄相對估值可以完美的念頭。

3）相對估值法是給資產定價，不是真正的估值。

注：本站所有學習達摩德仁教授課程的心得文章均已獲達摩德仁教授授權。
